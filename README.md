# FINANCE TRUST SACCO MS

FINANCE TRUST SACCO MS is a SACCO management platform with a React + TypeScript web application and a Django REST API. This package preserves the existing application and its business modules while updating its branding and deployment setup.

## Included modules

- Member profiles and KYC-related information
- Savings products, member accounts, balances, deposits and withdrawals
- Transaction history and account activity
- Loan applications, guarantors, documents, review, approval, rejection and disbursement workflows
- Role-based module access for administrators, managers, operations, finance, loan officers and accountants
- Operations dashboard with savings, transaction and loan insights

## Deployment architecture

**Vercel hosts the frontend only.** The Django API, PostgreSQL database and persistent KYC/loan-document storage must be hosted separately. Do not use SQLite or the Vercel filesystem for production financial data or uploaded documents. A Render Blueprint is included as a starter backend deployment in `render.yaml`; configure persistent object storage for uploaded documents before production use.

### 1. Deploy the Django API and PostgreSQL

1. Push this project to a private GitHub repository.
2. In Render, create a new Blueprint and select the repository. Render reads `render.yaml` and provisions the API service and PostgreSQL database.
3. Wait for the first deployment and copy the API service URL, for example `https://finance-trust-sacco-api.onrender.com`.
4. Set `CORS_ALLOWED_ORIGINS` and `CSRF_TRUSTED_ORIGINS` on the API service to your actual Vercel production domain (comma-separated if multiple).
5. Configure durable private object storage for `MEDIA_ROOT` and backups. The included Render setup does not by itself provide persistent media storage.

### 2. Deploy the frontend to Vercel

1. Import the same GitHub repository into Vercel.
2. Keep the project Root Directory at the repository root; `vercel.json` contains the frontend build settings.
3. Add the environment variable `VITE_API_URL` with the API base URL, including `/api/v1`, e.g. `https://finance-trust-sacco-api.onrender.com/api/v1`.
4. Deploy. Vercel runs `npm --prefix web-app ci` and `npm --prefix web-app run build`, then serves `web-app/dist`.
5. Add the final Vercel domain to the backend CORS and CSRF origin variables and redeploy the API.

### 3. Local development

Backend (Python 3.13 recommended):

```bash
cd backend
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
cp .env.example .env
python manage.py migrate
python manage.py runserver
```

Frontend (Node.js 20+):

```bash
cd web-app
npm ci
cp .env.example .env
npm run dev
```

The frontend is available at `http://localhost:3000`; the API is at `http://localhost:8000/api/v1/`; API health check: `http://localhost:8000/health/`.

## Important production checklist

- Set a strong unique `DJANGO_SECRET_KEY`; never use the local example.
- Use PostgreSQL and automated encrypted backups.
- Configure persistent private media/object storage and access controls for KYC documents.
- Set the real Vercel origin in CORS and CSRF settings.
- Create the first administrator with `python manage.py createsuperuser`; do not deploy demo credentials.
- Review role permissions, password reset/email delivery, audit logging, privacy notices and financial transaction controls before onboarding real members.
- MTN MoMo and Airtel Money are not enabled by this deployment configuration; configure official provider credentials and callback verification separately when ready.

## Project layout

```text
backend/       Django REST API
web-app/       React/Vite frontend
vercel.json    Vercel frontend build and SPA routing
render.yaml    Render API and PostgreSQL blueprint
USER_GUIDE.md  Staff workflow guide
```
