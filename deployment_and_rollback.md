# Manual Deployment and Rollback Guide

This document provides two example deployment-related issues for a Ruby on Rails-backed single-page application (SPA) supporting a simple online shopping website. Each issue includes manual resolution and rollback steps.

---

## 1. **Deployment Fails Due to Missing or Incorrect Environment Variables**

**Symptoms:** App crashes on startup, assets fail to compile, API requests break.

**Steps:**

1. Check deployment logs for missing variable errors.
2. Compare environment variable lists between previous and current deployments.
3. Validate `.env`, credentials, or secrets manager entries for typos.
4. Add or correct missing environment variables.
5. Restart the Rails app or server process.
6. Re-run asset compilation if required.
7. Verify SPA loads properly in browser.
8. Test major actions (login, add to cart, checkout).
9. If unresolved, revert to previous environment file snapshot.
10. Redeploy last-known working version.

---

## 2. **New Release Causes Checkout or Ordering Failures**

**Symptoms:** Orders remain in pending state, payment gateway errors appear after deployment.

**Steps:**

1. Inspect Rails logs for error stack traces from the checkout controller.
2. Compare request payloads or parameters with those in the previous release.
3. Validate that all database migrations were run successfully.
4. Roll back the most recent migration if errors correlate to schema changes.
5. Restart background job workers to clear crashed processes.
6. Perform a manual test checkout using test card data.
7. If issues persist, initiate rollback to previous application version.
8. Confirm old version is running cleanly and functionality restored.
9. Notify stakeholders of rollback completion.
10. Open a fix ticket and begin root-cause analysis on the broken release.
