# shyftplan API documentation

Every API call needs authentication with the params `user_email` and `authentication_token`.
The email is the one used for signing in into shyftplan. It is recommended to create a separate Login for using the API.

The authentication token can be aquired by two ways:

- a) Create an Authentication Token for your current employment on your Profile
- b) [POST] <https://shyftplan.com/api/v1/login?user[email]=email&user[password]=password>

     where `email` and `passord` are the credentials to log into shyftplan
     The response contains the Authentication token
     ```json
       {
         "success": true,
         "info": "Logged in",
         "authentication_token": ":token:",
         ...
       }
     ```