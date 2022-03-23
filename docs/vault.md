# Vault

## Login with an approle

### Requirements

Required env variables with `approle` informations:
* `VAULT_ROLE_ID`
* `VAULT_SECRET_ID`

Can be retrieve from Vault using:
```bash
VAULT_SERVICE_NAME="..."

VAULT_ROLE_ID=$(vault write -f auth/approle/role/$VAULT_SERVICE_NAME/role-id --format=json | jq -r .data.role_id)

VAULT_SECRET_ID=$(vault write -f auth/approle/role/$VAULT_SERVICE_NAME/secret-id --format=json | jq -r .data.secret_id)
```

### Login and read a secret 

```bash
# Login
vault write auth/approle/login role_id=$VAULT_ROLE_ID secret_id=$VAULT_SECRET_ID

# Then use the token retrieve in the last command output
vault login <output.token>
```

```bash
# Read secret
vault kv get kv/...
```