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
