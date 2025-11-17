## Hariyali Deployment Guide

### 1. Prerequisites
- Docker installed locally (for test builds).
- Git repository hosted on GitHub/GitLab.
- Accounts on Render (or similar container-friendly PaaS).
- Secrets: `FLASK_SECRET_KEY`, `WEATHER_API_KEY`.

### 2. Local build test (optional)
```bash
cd /Users/macbookpro/Desktop/capstone\ project\ /Hariyali
docker build -t hariyali .
docker run -p 10000:10000 --env FLASK_SECRET_KEY=change-me --env WEATHER_API_KEY=your-key hariyali
```
Visit `http://localhost:10000`.

### 3. Render deployment
1. Commit and push the repo, keeping `Dockerfile` and `render.yaml` at the root.
2. In Render dashboard choose **Blueprint** → **New Blueprint Instance** → link repo.
3. Review `render.yaml` defaults:
   - Uses Docker runtime.
   - Exposes port `10000`.
   - Auto deploys from main branch.
4. Provide environment variables under **Blueprint Variables**:
   - `FLASK_SECRET_KEY`: random string.
   - `WEATHER_API_KEY`: OpenWeather key.
5. Click **Apply**. Render builds the image with `Dockerfile` and starts `gunicorn`.
6. Once healthy, Render shows public URL `https://hariyali-app.onrender.com` (or similar) accessible worldwide.

### 4. Post-deploy checks
- Visit `/`, `/crop-recommend`, `/fertilizer`, `/disease` forms.
- Trigger inference paths to confirm model assets load.
- Monitor logs (`Deploys` → `View Logs`) for torch/model load issues.

### 5. Troubleshooting
- **Build timeouts**: upgrade plan or trim dependencies.
- **Model missing**: ensure `models/` included in repo.
- **Locale files**: update `BABEL_TRANSLATION_DIR` using env var if structure changes.

