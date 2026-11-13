# FINANCE TRUST SACCO MS — deployment checklist

## Vercel (frontend)
- Import this repository into Vercel with the repository root as the project root.
- The root `vercel.json` configures the Vite build and SPA fallback.
- Add `VITE_API_URL=https://YOUR-BACKEND-HOST/api/v1` in Vercel Environment Variables for Production (and Preview if needed).
- Trigger a deployment after setting the variable.

## Django API + PostgreSQL (Render blueprint included)
- Create a Render Blueprint from this repository; it reads `render.yaml`.
- Wait for the API and database to be provisioned.
- Set `CORS_ALLOWED_ORIGINS` and `CSRF_TRUSTED_ORIGINS` to the exact Vercel domain(s), comma-separated.
- Keep `DEBUG=False` and use the generated `DJANGO_SECRET_KEY`.
- Create the initial admin with `python manage.py createsuperuser` from the backend service shell.
- Configure persistent private object storage for uploaded KYC and loan files. The blueprint's container filesystem is not durable storage for member documents.
- Configure database backups, email delivery, monitoring and restore procedures before production use.

## Before real-money / live-member use
- Complete a security review of role permissions, authentication, audit trails, data retention and member consent.
- Verify all loan, savings, cash and reversal workflows with test accounts and reconcile reports.
- Do not add real member data until persistent encrypted document storage and backups are operational.
- Mobile money collections and withdrawals are not activated by this package. Integrate official MTN MoMo and Airtel Money APIs later using server-side secrets, signed callback validation, idempotency and reconciliation.

## Build verification
The source package and deployment configuration were checked for Python syntax and valid JSON configuration. Frontend dependency installation/build and Django runtime checks could not be completed in this environment because the package registries and Django dependencies were unavailable here. Vercel will run the configured clean install and production build during deployment; review its build log before promoting to production.
