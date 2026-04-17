# LESSONS.md — ARIA Mistakes & Learnings

Read this file every session startup. These are real mistakes made and how to avoid repeating them.

---

## Lesson 1 — Never confirm a fix without self-verification
**Mistake:** Said "fixed" and "done" multiple times for the footer logo white box issue without actually verifying the output file contained the correct change.
**Result:** Tony had to ask 5+ times for the same fix.
**Rule:** Before saying anything is fixed:
1. Read the actual file and confirm the change is present
2. Confirm the problem-causing code is removed
3. Only then say it is resolved
4. If cannot verify visually, say "pushed — please confirm on your end"

---

## Lesson 2 — PNG logos may have backgrounds that need removing
**Mistake:** Embedded the Iconic Investors logo PNG directly without removing its background, causing a white/black box to appear in the nav and footer.
**Root cause:** The logo PNG had a black background. After removing black pixels, the logo text (dark charcoal) became invisible on dark backgrounds.
**Solution:** 
- For dark backgrounds (footer): use a pure white version of the logo (all non-transparent pixels set to #ffffff)
- For light backgrounds (nav): use the original cleaned logo with transparent background
- Or use CSS `filter: brightness(0) invert(1)` to make dark logos white — but verify it works first
- Best fallback: use text logo on dark backgrounds to guarantee visibility

---

## Lesson 3 — Inline CSS !important overrides don't always win
**Mistake:** Added multiple CSS `!important` overrides via `</style>` injection, but inline styles in HTML elements were still winning in some browsers.
**Root cause:** Specificity battles between injected CSS and existing inline styles.
**Solution:** When overriding inline styles, edit the HTML attribute directly (change `style="..."` on the element) rather than fighting with CSS overrides.

---

## Lesson 4 — Duplicate section IDs break page navigation
**Mistake:** The HTML file had duplicate `id="about"`, `id="licensee-services"` etc. from the original Lovable source AND from our new sections.
**Result:** Nav links scrolled to wrong sections, About appeared at top of page.
**Solution:** Always check for duplicate IDs before adding new sections:
```python
import re
ids = re.findall(r'id="([^"]+)"', content)
duplicates = [id for id in ids if ids.count(id) > 1]
```

---

## Lesson 5 — GitHub Pages caches aggressively
**Mistake:** Pushed fixes and told Tony they were live, but GitHub Pages was serving cached version.
**Result:** Tony saw old broken version, thought fixes weren't applied.
**Solution:** 
- Always tell Tony to open in incognito/private window
- Or append `?v=X` to bust cache
- Never say "it's live" — say "pushed, open in incognito to see the latest"

---

## Lesson 6 — Test before sending emails
**Mistake:** Sent a test email to Bill Beimers (a real lead) using his live email address to test GHL email sending.
**Result:** Bill received a test email saying "⚠️ ARIA TEST — ignore this email" — unprofessional.
**Rule:** NEVER test email sending on real lead/contact email addresses. Always test on an internal address first (e.g. connect@iconicinvestors.com.au or a dummy address).

---

## Lesson 7 — No emails sent without Tony's explicit approval
**Mistake:** Sent test email to Bill Beimers without approval.
**Rule:** Zero outbound emails without explicit "yes, send it" from Tony. Draft → present → wait for approval → send. No exceptions, especially for the AFSL 430062 displaced adviser campaign.

---

## Lesson 8 — Always check Supabase before asking for credentials
**Mistake:** Asked Tony for credentials that were already stored in Supabase or TOOLS.md.
**Rule:** Before asking Tony for any credential:
1. Check `config` table in Supabase (`honrywzfzwmywwoaqevn`)
2. Check TOOLS.md
3. Check AGENTS.md
Only ask Tony if genuinely missing from all three.

---

## Lesson 9 — Fade-up animations hide content on page load
**Mistake:** Hero section content (badge, headline, buttons) and trust bar stats were invisible on mobile because fade-up animation started at `opacity: 0` and IntersectionObserver wasn't triggering for above-fold content.
**Solution:** Set `.fade-up { opacity: 1; transform: translateY(0); }` as default, or ensure IntersectionObserver fires for elements already in viewport on load.

---

## Lesson 10 — CSS variable corruption breaks entire design
**Mistake:** A subagent set `--green: #ffffff` (white) instead of `#1a3c2e` (forest green) when patching CSS.
**Result:** All green buttons became invisible (white on white), stat values disappeared, entire colour system broken.
**Rule:** After any CSS variable changes, immediately verify:
```python
import re
vars_block = re.search(r':root\s*\{[^}]+\}', content)
# Check --green is #1a3c2e, not #ffffff
```

---

## Lesson 11 — GHL API cannot update location email directly
**Mistake:** Tried to update GHL location notification email via API and got 401 Forbidden.
**Result:** Had to ask Tony to do it manually.
**Note:** Location-level settings in GHL require manual update via the GHL UI. API scope `locations.write` is restricted. Flag this to Tony and guide them through the UI steps.

---

## Lesson 12 — Never present work without verifying it works
**Mistake:** Deployed the React app to WordPress and told Tony it was live and working, without checking that the page actually rendered correctly. The page showed only a title — the JavaScript wasn't executing.
**Root cause:** WordPress strips script tags from page content. Embedding a React app in WordPress page content does not work.
**Rule:** Before telling Tony any deployment is done:
1. Open the URL myself (curl or fetch)
2. Verify the actual content renders as expected
3. Only then report it as complete
**Correct WordPress approach:** Upload React build files via FTP to public_html, not via WordPress page content API.

## Lesson 13 — Warn before DNS changes cause disruption
**Mistake:** Gave instructions to update DNS A records in Wix without warning that Hostinger would automatically migrate the domain folder structure when the domain was added, causing the staging URL to break and appearing to delete the site.
**Rule:** Before giving DNS change instructions:
1. Warn that adding a domain in Hostinger FIRST will migrate the file structure
2. Explain the staging URL will stop working once domain is added
3. Confirm files will be preserved — just at a new path
4. Give the full sequence in the correct order with warnings at each step

## Lesson 17 — Check before acting, confirm before presenting
**Rule:** Before taking any action:
1. Research and verify the approach is correct
2. Test/confirm the output is what was asked for
3. Only then act and present
Never assume. Never present work that hasn't been verified.

## Lesson 16 — Complete jobs or explain why not
**Rule:** If given a job, complete it. If unable to complete it, immediately explain:
1. What is blocking completion
2. What is needed to unblock it
3. What has been done so far
No silent failures. No half-done work presented as done.

## Lesson 15 — Research before advising
**Mistake:** Gave DNS instructions without fully verifying the consequences. Hostinger migrating the domain folder was a known side effect that was not communicated.
**Rule:** Before giving any technical advice:
1. Research and verify it is correct
2. Check all consequences and side effects
3. Give the full sequence in the right order
4. Only advise once confident it is right
No half-baked instructions. No advice that costs Tony time.

## Lesson 14 — Every change must be saved to GitHub immediately
**Mistake:** Multiple website deployments were made without consistently saving the source code to GitHub first. Memory and config files were not always synced.
**Rule:** 
- Before deploying anything to the server, commit to GitHub first
- Every website version = a GitHub commit in tony583/iconic-assets
- Every session = sync to tony583/aria-memory at the end
- If it's not in GitHub, it doesn't exist
- AGENTS.md, SOUL.md, LESSONS.md, MEMORY.md, TOOLS.md — all must be current in GitHub

*Last updated: 2026-04-17*
*Add new lessons as they occur — never delete old ones*
