
## Auth0 server
- admin  http://127.0.0.1:4445/
- server http://127.0.0.1:4444/
- Authen server http://127.0.0.1:5555/callback

## client credentials grant

### create client
```
client=$(docker compose -f quickstart.yml exec hydra \
    hydra create client \
    --endpoint http://127.0.0.1:4445/ \
    --format json \
    --grant-type client_credentials)

# We parse the JSON response using jq to get the client ID and client secret:
client_id=$(echo $client | jq -r '.client_id')
client_secret=$(echo $client | jq -r '.client_secret')

```
### perform token issue
```
docker compose -f quickstart.yml exec hydra \
  hydra perform client-credentials \
  --endpoint http://127.0.0.1:4444/ \
  --client-id $client_id \
  --client-secret $client_secret
```

## OAuth 2.0 Authorization Code Grant
### create client
- need a callback for each client
```
code_client=$(docker compose -f quickstart.yml exec hydra \
    hydra create client \
    --endpoint http://127.0.0.1:4445 \
    --grant-type authorization_code,refresh_token \
    --response-type code,id_token \
    --format json \
    --scope openid --scope offline \
    --redirect-uri http://127.0.0.1:5555/callback)

code_client_id=$(echo $code_client | jq -r '.client_id')
code_client_secret=$(echo $code_client | jq -r '.client_secret')
```
### perform token issue



## introspection

```
docker compose -f quickstart.yml exec hydra \
  hydra introspect token \
  --format json-pretty \
  --endpoint http://127.0.0.1:4445/ \
  UDYMha9TwsMBejEvKfnDOXkhgkLsnmUNYVQDklT5bD8.ZNpuNRC85erbIYDjPqhMwTinlvQmNTk_UvttcLQxFJY
```