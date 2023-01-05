### GitLab integration

1. Having a Vault namespace

2. Configure Vault access
```bash
export VAULT_ADDR="..."
export VAULT_NAMESPACE="sku-test-911ded6c3fc84e86be3e636f87fbb35f/terraform-statuspage"
```

(need a token)

3. Enable the JWT Auth method
```bash
vault auth enable jwt
```

4. Config JWT Auth method for gitlab service
```bash
GL_FQDN="..."
GL_JWKS_URL=".../-/jwks"

vault write auth/jwt/config \
  jwks_url=${GL_JWKS_URL} \
  bound_issuer=${GL_FQDN}
```

5. Enable KV Secrets Engine
```bash
vault secrets enable -version=2 kv
``` 


# Create a default policy to read kv data
```bash
vault policy write read-kv-credentials - <<EOF
# Read-only permission on 'kv/data/credentials/*' path

path "kv/data/credentials/*" {
  capabilities = [ "read" ]
}
EOF
```

Define a role and limit it to a project path
* https://gitlab-ncsa.ubisoft.org/it/rome/palatine/observability/statuspagectl (`123548`)
```bash
vault write auth/jwt/role/statuspagectl-ci - <<EOF
{
  "role_type": "jwt",
  "policies": ["read-kv-credentials"],
  "token_explicit_max_ttl": 60,
  "user_claim": "user_email",
  "bound_claims_type": "glob",
  "bound_claims": {
    "project_path": "it/.../observability/terraform-*"
  }
}
EOF
```


Define a role and limit it to a specific project
```bash
vault write auth/jwt/role/statuspagectl-ci - <<EOF
{
  "role_type": "jwt",
  "policies": ["read-kv-credentials"],
  "token_explicit_max_ttl": 60,
  "user_claim": "user_email",
  "bound_claims_type": "glob",
  "bound_claims": {
    "project_id": "(126723|128468)"
  }
}
EOF
```