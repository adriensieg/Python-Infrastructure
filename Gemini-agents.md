
```mermaid
flowchart TD
  %% Top-level actors
  subgraph Identities
    A1[Azure Entra / Azure AD (User: Adrien)]
    A2[Other End Users]
  end

  subgraph Google_WIF["Google (Account A) - Workforce Identity Federation"]
    WIF[Workforce Identity Pool\n(e.g. mcd-agentspace-wif-pool)]
    PRINCIPAL["WIF Principal Set\nprincipalSet://.../mcd-agentspace-wif-pool/*"]
  end

  subgraph Gemini["Gemini Enterprise (Account A)"]
    G_UI[Gemini UI / Data Insights Agent]
    G_OAUTH[OAuth Client (Web App)\nclient_id / client_secret\nredirect_uri]
    G_Service[Gemini Backend\n(token store, NL model, planner)]
    G_AgentConfig[Agent Configuration\n(Data Insights agent)]
    G_DB[(Gemini internal DB for\nrefresh tokens & config)] 
  end

  subgraph Google_OAuth["Google OAuth / Accounts"]
    OAuthAuth["Authorization Endpoint\nhttps://accounts.google.com/o/oauth2/v2/auth"]
    OAuthToken["Token Endpoint\n/oauth2/v4/token"]
    GoogleSession["Google session (federated)"]
  end

  subgraph BigQuery["BigQuery / Data (Account B)"]
    BQ_ProjectB["Project B\ndatasets/tables"]
    BQ_API["BigQuery API\njobs.insert, datasets.list, tables.list"]
    BQ_Data["Datasets & Tables\n(project B)"]
    BQ_DSACL["Dataset ACLs / IAM bindings"]
  end

  subgraph GCP_IAM["Google Cloud IAM & Security Controls"]
    IAM_Bindings["IAM grants (org/folder/project/dataset)\nroles/bigquery.dataViewer, jobUser, metadataViewer"]
    SA["Service Accounts (optional)"]
    SA_Impersonation["Service Account Impersonation\n(roles/iam.serviceAccountTokenCreator)"]
    VPCSC["VPC Service Controls (optional)"]
    CMEK["CMEK / KMS"]
    IAM_Conditions["IAM Conditions"]
    OAuthConsent["OAuth Consent Verification\nSensitive scopes"]
  end

  subgraph Logging["Observability"]
    CloudLogging["Cloud Audit Logs\n(bigquery.jobs.insert etc.)"]
    OAuthAudit["OAuth Grant Records (Admin Console)"]
    AccessTransparency["Access Transparency / Access Approval (optional)"]
  end

  %% Setup flows
  A1 -->|SAML/OIDC federation| WIF
  A2 -->|SAML/OIDC federation| WIF

  WIF --> PRINCIPAL
  PRINCIPAL -->|Assign IAM roles in Projects| IAM_Bindings
  IAM_Bindings -->|Can be granted at org/folder/project/dataset| BQ_DSACL
  IAM_Bindings -->|Can include service account impersonation| SA_Impersonation
  IAM_Bindings --> SA

  %% Gemini setup
  G_UI --> G_AgentConfig
  G_AgentConfig --> G_OAUTH
  G_AgentConfig -->|Grant product-role| PRINCIPAL
  PRINCIPAL -->|Product role: Agent User| G_UI

  OAuthAuth -.->|user consent screen\n(scope: https://www.googleapis.com/auth/bigquery)| OAuthConsent

  %% Interactive OAuth flow (Model A - user delegated OAuth)
  A1 -->|Open Gemini UI & click connect BigQuery| G_UI
  G_UI -->|Redirect (auth request)\nclient_id, scope, redirect_uri, response_type=code, access_type=offline| OAuthAuth
  OAuthAuth -->|User authenticates (federated)\nAzure AD -> WIF -> Google session| GoogleSession
  GoogleSession -->|Consent| OAuthAuth
  OAuthAuth -->|Authorization Code| G_UI
  G_UI -->|Exchange code for tokens\n(client_id, client_secret)\nPOST /oauth2/v4/token| OAuthToken
  OAuthToken -->|access_token + refresh_token (user-scoped)| G_Service
  G_Service -->|Store refresh_token securely| G_DB

  %% Gemini enumerates datasets using user token (Model A)
  G_Service -->|Call BigQuery datasets.list / tables.list\nAuthorization: Bearer <user_access_token>| BQ_API
  BQ_API -->|Check IAM policies| BQ_DSACL
  BQ_API -->|Return only projects/datasets user has access to| G_Service
  G_Service -->|Show dataset list in Gemini UI| G_UI

  %% User runs a query (Model A) - runtime
  G_UI -->|User asks NL question| G_Service
  G_Service -->|Generate SQL / generate job| G_Service
  G_Service -->|Ensure access_token valid\nif expired -> use refresh_token to get new access_token| OAuthToken
  G_Service -->|POST bigquery.jobs.insert (SQL)\nAuthorization: Bearer <user_access_token>| BQ_API
  BQ_API -->|Execute job (on project where job runs)\nCheck bigquery.jobUser & dataViewer| BQ_DSACL
  BQ_API -->|Return query results| G_Service
  G_Service -->|Render results & NL answer| G_UI

  %% Auditing when Model A used
  BQ_API -->|Write logs| CloudLogging
  CloudLogging -->|protoPayload.authenticationInfo.principalEmail = user@example.com\n(proto shows end-user principal)| Logging
  OAuthToken -->|OAuth grant recorded| OAuthAudit

  %% Alternative: Model B - service account (or impersonation)
  subgraph ModelB["Model B: Service Account or Impersonation (Alternate path)"]
    MB1[Gemini uses Service Account creds\n(or impersonates SA)]
  end

  G_Service -->|Optionally use SA or impersonate SA| MB1
  MB1 -->|If impersonation: Gemini uses\nworkforce-principal or backend SA to call\niam.serviceAccounts.generateAccessToken| SA_Impersonation
  MB1 -->|Call BQ API using SA access token| BQ_API
  BQ_API -->|Return datasets (based on SA IAM)\n(all users see same lists)| G_Service
  BQ_API -->|Write logs| CloudLogging
  CloudLogging -->|protoPayload.authenticationInfo.principalEmail = service-account@project.iam.gserviceaccount.com| Logging

  %% Security controls & mitigations (side nodes)
  G_Service ---|store tokens securely| G_DB
  G_DB -.->|Enable KMS/CMEK for secrets| CMEK
  VPCSC -.->|if enabled restricts external SaaS access| BQ_API
  IAM_Conditions -.->|attach conditions to role grants| IAM_Bindings
  OAuthConsent -.->|sensitive scope verification may be required| OAuthAuth

  %% Admin actions / checks
  subgraph Admin["Admin consoles / Investigations"]
    AdminConsole1[Google Cloud Console\nIAM & Admin]
    AdminConsole2[Cloud Logging Viewer]
    AdminConsole3[Cloud Identity / Workspace Admin\nOAuth app approvals]
  end

  AdminConsole1 -->|Check IAM bindings\nsearch for workforce principal| IAM_Bindings
  AdminConsole2 -->|Query logs for bigquery.jobs.insert\nfilter by requestor / app| CloudLogging
  AdminConsole3 -->|View which users granted OAuth to Gemini\nrevoke refresh tokens| OAuthAudit

  %% End-to-end summary flow line (compact)
  A1 --> G_UI --> OAuthAuth --> OAuthToken --> BQ_API --> G_UI

  %% Legend / notes
  classDef note fill:#f9f,stroke:#333,stroke-width:1px;
  N1["Note: 'Principal seen in logs' indicates whether\ncalls used the end-user or a service account"]:::note
  Logging -.-> N1

  %% Visual grouping for setup vs runtime
  subgraph Setup["SETUP (Admin)"]
    WIF
    PRINCIPAL
    IAM_Bindings
    G_OAUTH
    G_AgentConfig
    G_Service
  end

  subgraph Runtime["RUNTIME (when user queries)"]
    A1
    G_UI
    OAuthAuth
    OAuthToken
    BQ_API
    CloudLogging
  end

  %% Additional conditional controls and signals
  BQ_API -->|If Row-Level Security / Column policies applied\nreturn filtered results| BQ_Data
  BQ_DSACL -->|Authorized views / dataset ACLs| BQ_Data

  %% Make sure arrows are clear
  style G_DB fill:#fff3bf,stroke:#9a7d00
  style CloudLogging fill:#e6f2ff,stroke:#2a6f9e
  style VPCSC fill:#f0f0f0,stroke:#888
  style CMEK fill:#f0f0f0,stroke:#888

```
