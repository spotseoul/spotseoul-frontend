# Frontend standalone setup

## Run locally

```bash
cd frontend
npm install
cp .env.example .env
npm run dev
```

## Environment variables

- VITE_API_BASE_URL: backend API base URL used by the Vue app
- VITE_API_PROXY_TARGET: development proxy target for Vite

## Deploying independently

- The frontend reads the API URL from Vite environment variables.
- For production, set VITE_API_BASE_URL to the deployed backend URL.
- The Vite dev server binds to 0.0.0.0 and proxies /api requests to the backend.
