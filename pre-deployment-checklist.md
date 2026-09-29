# Pre-Deployment Checklist

Before pushing code to a production environment, complete the following verification steps to ensure security, performance, and reliability.

---

## 🔒 1. Security & Configuration
- [ ] **Environment Variables (`.env`)**: Ensure `.env` is listed in `.gitignore`. Verify that no API keys, database connection strings, or JWT secrets are hardcoded in source files.
- [ ] **CORS Configuration**: Replace wildcard origin settings (`cors()`) with explicit production front-end origins (e.g., `origin: "https://my-frontend.vercel.app"`).
- [ ] **HTTP Headers (`helmet`)**: Use the `helmet` middleware package to hide sensitive stack headers (like `X-Powered-By: Express`) and enable basic protections against XSS and clickjacking.
- [ ] **Rate Limiting**: Implement `express-rate-limit` on public API endpoints (especially login/auth routes) to prevent brute-force and Denial-of-Service (DoS) attacks.

---

## 🗄️ 2. Database Management
- [ ] **Production Migrations**: For SQL/Prisma setups, run `npx prisma migrate deploy` in your production build script rather than `db push` to apply schema changes safely without data loss.
- [ ] **Indexing**: Verify that high-query database fields (like `email`, `username`, or foreign keys) have database indexes applied to prevent slow table scans under load.
- [ ] **Connection Pooling**: Configure database connection limits appropriately for free/low-tier database limits so server instances do not exhaust available connections.

---

## 🚨 3. Error Handling & Logging
- [ ] **Hiding Stack Traces**: Implement a centralized Express error-handling middleware that conditionally returns error stack traces only in development (`NODE_ENV === 'development'`). Send sanitized error messages to public users in production.
- [ ] **Production Logging**: Use structured logging libraries (such as `pino` or `winston`) or rely on platform stdout console logging (`console.error`). Avoid excessive `console.log` statements in hot code paths.
- [ ] **Live Log Access**: Verify access to live production logs via the hosting platform's dashboard (e.g., Render Dashboard -> **Logs** tab) for real-time monitoring and debugging.

---

## 🧹 4. Environment & Build Optimization
- [ ] **Dependencies Cleanup**: Move local tools (`nodemon`, `jest`, `prisma` CLI) to `devDependencies` in `package.json` so production builds install only essential runtime modules (`npm install --production`).
- [ ] **Dynamic Port Binding**: Ensure Express listens on `process.env.PORT || 5000` rather than a hardcoded port number, allowing the hosting platform to assign its dynamic port.
- [ ] **Health Check Endpoint**: Provide a simple `GET /health` or `GET /api/status` route returning a `200 OK` response so platform health checks can verify the service is operational.