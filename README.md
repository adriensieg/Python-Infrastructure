
# To do list of things to learn

## Cybersecurity
https://hackerassociate.com/offensive-redteam-professional-ortp-training-program/

## 

https://mcd-tools.atlassian.net/wiki/spaces/~712020a7efa1a043ed4fab9fab694145b7b798/pages/368808450/From+Vulnerable+to+Vigilant+Catching+Security+Breaches+in+Your+Web+App

OAuth: https://mcd-tools.atlassian.net/wiki/spaces/~712020a7efa1a043ed4fab9fab694145b7b798/pages/383131431/OAuth+2.0+from+scratch
https://mcd-tools.atlassian.net/wiki/spaces/~712020a7efa1a043ed4fab9fab694145b7b798/pages/491852529/Real-Time

## Create a Daily basic Flask Application

- Logging
- BluePrint
- HEADERS
- CORS
- async when api
- Pydantic
- Try / except
- except requests.exceptions.RequestException as e:



Behind LB
- Change all your endpoints OR window
- <link rel="stylesheet" href="{{ url_for('static', filename='styles.css', _scheme='https') }}">
- app = Flask(__name__, static_url_path='/audio-uploader/static')
- # Add this configuration to force HTTPS for static files
app.config['PREFERRED_URL_SCHEME'] = 'https'
- @app.route('/audio-uploader/')
- return jsonify({'error': 'No selected file'}), 400
- return jsonify({
           'message': 'File uploaded successfully',
           'filename': unique_filename
       }), 200

- This is a classic mixed content security issue. The problem occurs because your page is being served over HTTPS (https://frenglish.me/audio-uploader/), but some of your resources (CSS, images, etc.) are being requested over HTTP. Modern browsers block mixed content for security reasons.

Let's fix this by ensuring all content is served over HTTPS. The issue appears to be in how the static resources are being referenced in your templates.
