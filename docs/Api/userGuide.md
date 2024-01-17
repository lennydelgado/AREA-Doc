
# Area Backend API Documentation

  

## Introduction

  

The Area Backend API allows users to configure and execute automation services. Users can manage their account, connect to third-party services, and define automation rules.

  

## Authentication

- Endpoint: **/users/register**

- Method: POST

- Description: Register a new user.

- Parameters: User registration data.

- Endpoint: **/users/login**

- Method: POST

- Description: Authenticate a user.

- Parameters: User credentials.

- Endpoint: **/users/update-password**

- Method: POST

- Description: Update user password.

- Parameters: New password.

  

## User Management

- Endpoint: **/users/read**

- Method: GET

- Description: Retrieve user information.

- Authentication: Requires valid user token.

  

## Third-party Integrations

- Endpoint: **/api/spotify-credentials**

- Method: GET

- Description: Retrieve Spotify API credentials.

- Authentication: Requires valid user token.

  

- Endpoint: **/login/google**, **/google/callback**, **/login/facebook**, **/facebook/callback**

- Description: OAuth2 login and callback for Google and Facebook.

  

## Email Service

- Endpoint: **/send-email**

- Method: POST

- Description: Send an email.

- Parameters: Email content.

- Authentication: Requires valid user token.

  

## Services Management

- Endpoint: **/services/read**

- Method: GET

- Description: Get configured services.

- Authentication: Requires login.

  

- Endpoint: **/services/add**

- Method: POST

- Description: Add a new service.

- Parameters: Service details.

- Authentication: Requires login.

  

- Endpoint: **/services/delete**

- Method: PUT

- Description: Delete a service.

- Parameters: Service ID.

- Authentication: Requires login.

  

## Automations

- Endpoint: **/automations/create**

- Method: POST

- Description: Create a new automation.

- Parameters: Automation details.

- Authentication: Requires valid user token.

  

- Endpoint: **/automations/read**

- Method: GET

- Description: Get user's automation rules.

- Authentication: Requires valid user token.

  

- Endpoint: **/automations/modify**

- Method: PUT

- Description: Modify an existing automation.

- Parameters: Automation details.

- Authentication: Requires valid user token.

  

- Endpoint: **/automations/status**

- Method: PUT

- Description: Change automation status (enable/disable).

- Parameters: Automation ID and status.

- Authentication: Requires valid user token.

  

- Endpoint: **/automations/delete**

- Method: DELETE

- Description: Delete an automation.

- Parameters: Automation ID.

- Authentication: Requires valid user token.

  

## Discord Integration

- Endpoint: **/discord/oauth2/callback**

- Method: GET

- Description: Callback for Discord OAuth2.

- Authentication: Requires valid user token.


## Available Automations and type of Services

### Actions trigger
**Timer:**
type: ‘timer’  
- Check everyday for a specific hour and minute specified. Will trigger the reactions at the correct time.
action: ‘everyday’  
detail1: ‘hour’  
detail2: ‘minute’

- Check for a specific day, hour and minute specified.  Will trigger the reactions at the correct time.
action: ‘day'  
detail1: ‘day’  
detail2: ‘hour’  
detail3: ‘minute’

**Spotify:**
type: ‘spotify’  
- Am I listening to a song trigger
action: ‘is_listening’

### Reactions:  
**Spotify:**  
type: 'spotify'
- Create a playlist
action: ‘create_playlist’  
detail1: ‘name’  
detail2: true/false (public/private)  
detail3: ‘description’

- Add music to playlist  
action: ‘add_to_playlist’  
detail1: ‘playlist name’  
detail2: 'artist of the song'  
detail3: 'name of the song'

- Delete music from playlist
action: ‘delete_song_from_playlist’  
detail1: ‘playlist name’  
detail2: ‘artist of the song'  
detail3: 'name of the song’

- Generate a recommended playlist for the user and append it to an existing playlist
action: ‘create_recommended’  
detail1: ‘playlist name’

**Discord:**
type: ‘discord’  
- Send a message to a channel  
action: ‘send_message’  
detail1: channel url’  
detail2: ‘the message'

**Gmail:**
type ‘gmail  
- Send an email using smtp  
action ‘send_email_smtp’  
detail1: ‘destinataire’  
detail2: ‘object’  
detail3: ‘contenu’
