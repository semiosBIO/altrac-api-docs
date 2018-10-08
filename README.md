# Altrac API Documentation
## Authentication
Currently there is one way to authenticate to the Altrac API. 
### User Credential Login
```
curl --request POST \
  --url https://altrac-api.com/users/login \
  --header 'content-type: application/json' \
  --data '{
  "email": "{email}",
  "password": "{password}"
}'
```