# Altrac API Documentation
## Authentication
Currently there is one way to authenticate to the Altrac API. 
### User Credential Login
Request:
```Shell
curl --request POST \
  --url https://altrac-api.com/users/login \
  --header 'content-type: application/json' \
  --data '{
  "email": "{email}",
  "password": "{password}"
}'
```
Response:
```JSON
{
  "id": "{id}",
  "email": "{email}",
  "first_name": null,
  "last_name": null,
  "company": null,
  "position": null,
  "country": null,
  "state": null,
  "city": null,
  "main_phone": null,
  "secondary_phone": null,
  "address": null,
  "zip_code": null,
  "customer_id": null,
  "token": "{token}"
}
```
You will be able to use the token generated in the response in order to make the other documented queries.