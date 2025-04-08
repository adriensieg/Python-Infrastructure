
```mermaid
flowchart TD

  %% User Entry Points
  A[User Initiates Conversation] --> B{First Time User?}
  
  %% First-time user vs Returning user handling
  B -- Yes --> C[Deterministic: Welcome Message]
  B -- No --> D[Context Awareness: Load User History, Memory]
  
  %% Understanding User Intent
  C --> E{User Intent Clear?}
  D --> E

  %% If intent is clear, process it
  E -- Yes --> F[Intent Processing: Identify Intent, Entities, Context Variables]
  E -- No --> G[Fallback Handling: Rephrase Request, Provide Options]

  %% Handling fallback scenarios
  G --> Q{Multiple Failures?}
  Q -- Yes --> M[Escalate to Human Agent or Alternative Channel]
  Q -- No --> R[Clarification Request: Ask User to Provide More Details]
  R --> E

  %% Processing recognized intent
  F --> H{Complex Query?}
  H -- Yes --> I[Generative AI Response: LLM, LangChain, Multi-Turn Management]
  H -- No --> J[Deterministic Response: Predefined Answers, State Machine]

  %% Confidence Check for Response
  I --> K{Confidence Check: Is Response Reliable?}
  J --> K
  K -- High --> L[Send Response]
  K -- Low --> M

  %% Managing Session Continuity and Follow-Ups
  L --> N{Follow-up Required?}
  N -- Yes --> O[State Machine: Move to Next State]
  N -- No --> P[Session Storage: Persist Data for Future Interactions]

  %% External Integrations
  O --> Y{Need External API or Database Query?}
  Y -- Yes --> Z[Trigger Webhook/API Call: Retrieve Data]
  Y -- No --> AA[Continue Normal Conversation Flow]

  %% Personalization and Memory Usage
  P --> BB{Use Past Conversations?}
  BB -- Yes --> CC[Retrieve Long-Term Memory for Personalization]
  BB -- No --> DD[Discard Context, Start Fresh]

  %% Escalation Handling
  M --> EE{Human Available?}
  EE -- Yes --> FF[Hand off to Human Agent]
  EE -- No --> GG[Schedule Callback or Offer Help Center Resources]

  %% Parallel subgraphs
  subgraph "Context Management"
      S[Session Variables: Store Context Data]
      T[User History: Access Prior Interactions]
      U[Current State: Track Ongoing Conversation]
  end

  subgraph "Response Generation"
      V[Templates: Predefined Responses]
      W[Dynamic Content: Adaptive Responses]
      X[External APIs: Real-Time Data Retrieval]
  end

  subgraph "Fallback & Error Handling"
      HH[Clarification Requests]
      II[Rephrase and Retry]
      JJ[Alternative Response Strategies]
  end
```


```mermaid
graph LR
    A[Start] --> B{Crew Member Staffed at Present Window 2?};
    B -- Yes --> C{Order Ready?};
    B -- No --> X[Initial Decision: Direct car to Window 1 for Cash];

    C -- Yes --> D{Car at Window 1?};
    C -- No --> E{Car at Window 1?};

    D -- No --> F[Direct car to Window 1 for Cash & Present];
    D -- Yes --> F

    E -- Yes --> G{Car at Window 2?};
    E -- No --> H{Car at Window 2?};

    G -- No --> I[Indicate to customer: "Please wait for window assignment" until car in window 1 starts to move, then Direct car to Window 1 if order is ready or Window 2 if not ready];
    G -- Yes --> I

    H -- No --> J[Direct car to Window 2 for Cash & Present];
    H -- Yes --> K[Indicate to customer: "Please wait for window assignment" until car in window 1 starts to move, then Direct car to Window 1 if order is ready or Window 2 if not ready];
    
    X --> Y{Order Ready?};
    Y -- Yes --> Z[Direct the car to Window 1 for Cash];
    Y -- No --> Z

    F --> L[Reprioritization Needed?];
    I --> L
    J --> L
    K --> L
    Z --> M[Reprioritization Needed?];

    L -- Yes --> N{Car pays at Window 1, order not ready, car at Window 2 leaves, car 1 spot before Present Window 1?};
    M -- Yes --> O{Car pays at Window 1, order not ready, no car at Window 2 or car at Window 2 leaves, car 1 spot before Present Window 1?};
    L -- No --> P[End];
    M -- No --> P

    N -- Yes --> Q[Direct car at Window 1 to pull forward to Present Window 2];
    N -- No --> P
    O -- Yes --> Q
    O -- No --> P

    Q --> P
```
