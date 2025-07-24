## 🛠️ Workflow-Schritte (Ablauf)

### 1. Code Checkout

```
- uses: actions/checkout@v4
```

---

### 2. SSH-Konfiguration

```
mkdir -p ~/.ssh
echo "${{ secrets.SSH_PRIVATE_KEY_RUNNER }}" > ~/.ssh/id_rsa
chmod 600 ~/.ssh/id_rsa
ssh-keyscan -H your-server.com >> ~/.ssh/known_hosts
```

- SSH-Zugang über GitHub Secret
- Verbindung zu dediziertem SSH-User (z. B. `github-runner`)

---

### 3. Google Cloud Auth – Workload Identity Federation

```
- uses: google-github-actions/auth@v2
  with:
    workload_identity_provider: projects/.../providers/github-provider
    service_account: calendar-auth@your-project.iam.gserviceaccount.com
    token_format: "access_token"
```

- Zugriff auf Google Calendar API via OIDC
- Access Token wird später im Deployment als ENV-Variable gesetzt

---

### 4. Build mit Nx

```
npx nx run-many --target=build --all
```

- Baut alle Services & Frontends
- Output landet in `dist/`

---

### 5. Remote Deployment via SSH + SCP

```
scp -r dist/apps/ai-service/. github-runner@your-server.com:/srv/ai-service

ssh github-runner@your-server.com << 'EOF'
  cd /srv/ai-service
  npm install
  npx sequelize-cli db:migrate --env production
  pm2 restart ai-service
EOF
```

- Jeder Service hat ein eigenes Deployment-Zielverzeichnis (`/srv/<service>`)
- DB-Migrationen & Prozessmanagement laufen automatisch

---

### 6. Google Token auf Server setzen

```
ssh github-runner@your-server.com \
"echo 'GOOGLE_ACCESS_TOKEN=${{ steps.auth.outputs.access_token }}' >> /srv/env/lead-service.env.prod"
```

- Google Access Token wird zur Laufzeit erzeugt und in das ENV-File geschrieben

---
