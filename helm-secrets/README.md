# Installation
https://github.com/jkroepke/helm-secrets/wiki/Installation

```
wget https://github.com/jkroepke/helm-secrets/releases/download/v4.4.2/helm-secrets.tar.gz
tar xvfp helm-secrets.tar.gz
helm plugin install helm-secrets
```

# Integration to ArgoCD
https://github.com/jkroepke/helm-secrets/wiki/ArgoCD-Integration

```
# https://fluxcd.io/flux/guides/mozilla-sops/
gpg --batch --full-generate-key <<EOF
%no-protection
Key-Type: 1
Key-Length: 4096
Subkey-Type: 1
Subkey-Length: 4096
Expire-Date: 0
Name-Comment: sops
Name-Real: sops
EOF

gpg --list-secret-keys
gpg --armor --export-secret-keys <key-id> > key.asc
kubectl -n argocd create secret generic helm-secrets-private-keys --from-file=key.asc=key.asc

kubectl -n argocd apply -k ../argocd/
```

# Install SOPS
https://github.com/mozilla/sops#stable-release

# Using SOPS
```
export SOPS_PGP_FP="<key-id>"

cat <<EOF > secret.yaml
apiVersion: v1
kind: Secret
metadata:
  name: test-secret
data:
  foo: bar
EOF

sops --encrypt ./secret.yaml > ./secret.enc.yaml
sops -d ./secret.enc.yaml
```

# Using SOPS with ArgoCD
TODO
