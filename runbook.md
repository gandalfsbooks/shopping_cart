# Runbook: Common Issues in a Ruby on Rails Single‑Page Application (SPA)

This runbook covers five frequent issues encountered in a Rails‑backed SPA for an online shopping site, along with clear steps to diagnose and resolve each.

---

## 1. **SPA Not Loading / White Screen on Initial Load**

**Symptoms:** Blank page, console errors, JS bundle not loading.

**Steps:**

1. Open browser DevTools → Console for JS errors.
2. Check that the Webpacker/Vite build succeeded (`bin/webpack` or equivalent).
3. Verify the asset manifest exists and is referenced properly.
4. Ensure `yarn install` or `npm install` has been run.
5. Clear Rails cache: `rails dev:cache` or `rails tmp:clear`.
6. Restart Rails server and asset watcher.
7. Check for missing environment variables required by frontend.
8. Inspect network tab for failed asset requests (404/500).
9. Confirm `config.public_file_server.enabled` is set correctly in production.
10. Rebuild assets and redeploy.

---

## 2. **API Requests Failing with 401/403 Errors**

**Symptoms:** Login not working, cart actions fail, unauthorized responses.

**Steps:**

1. Confirm user session or token is present in requests.
2. Inspect request headers for missing CSRF or Authorization tokens.
3. Validate Rails CSRF handling (`protect_from_forgery`) isn't blocking SPA.
4. Check session store configuration (Redis, cookie store, etc.).
5. Verify token expiration and refresh logic.
6. Inspect Rails logs for authentication failures.
7. Ensure CORS settings allow SPA origin.
8. Re‑enable or rotate secrets if corrupted.
9. Test authentication manually via `curl`.
10. Restart authentication backend if external (e.g., OAuth provider).

---

## 3. **Slow Page Loads or Sluggish Cart Updates**

**Symptoms:** Delays loading product lists, cart interactions lag.

**Steps:**

1. Use browser Network tab to identify slow endpoints.
2. Check Rails logs for long-running queries.
3. Add indices to common query columns (e.g., product SKU, user_id).
4. Ensure N+1 queries are eliminated using `includes`.
5. Profile memory/CPU usage on the Rails server.
6. Review caching (fragment cache, Redis, CDN) effectiveness.
7. Confirm background jobs are running (Sidekiq/Resque).
8. Verify database connection pool capacity.
9. Optimize image sizes for product thumbnails.
10. Test performance after deploying fixes.

---

## 4. **Checkout Fails Due to Payment Gateway Errors**

**Symptoms:** Payment not processed, gateway returns 400/500 errors.

**Steps:**

1. Check logs for exact gateway error message.
2. Verify payment API keys are set correctly in environment.
3. Confirm the app is hitting the correct mode (test vs production).
4. Ensure SSL/HTTPS is enforced for secure payment flows.
5. Validate required parameters being sent to the gateway.
6. Test a payment directly using gateway's CLI or dashboard tools.
7. Check network security groups/firewalls for outbound restrictions.
8. Rotate API keys if suspected compromise.
9. Retry with a different card/token to rule out client data issues.
10. Contact payment provider if gateway downtime suspected.

---

## 5. **Rails Background Jobs Stuck or Not Processing Orders**

**Symptoms:** Orders stay in pending state, emails not sent.

**Steps:**

1. Check job queue dashboard (Sidekiq/Resque/DelayedJob).
2. Verify workers are running and not in a crash loop.
3. Inspect logs for failed or retried jobs.
4. Ensure Redis or job backend is reachable.
5. Confirm queue names match those used in `perform_later` calls.
6. Check memory/CPU pressure on worker machines.
7. Restart job workers.
8. Replay dead jobs after verifying they’re safe to run.
9. Deploy job‑related code changes if recently modified.
10. Monitor for resumed processing.

---

**End of Runbook**
