# HEARTBEAT.md — ARIA Active Tasks

## Memory Maintenance (EVERY HEARTBEAT)
Check if today's memory file exists and is up to date.
- File: `memory/YYYY-MM-DD.md`
- Create if missing, update if stale (last update > 1 hour ago)
- Log a summary of what has been discussed and done
- Update MEMORY.md if anything significant happened

## GHL Pipeline Check (2x per day)
- Check for new leads in GHL
- Check for leads needing follow-up (no contact in 48h)
- Flag any stale opportunities to Tony
- API: GET https://services.leadconnectorhq.com/contacts/ with GHL API key

## Compliance Watch (DAILY)
- Check AR files for expiring licences or missing CPD
- Flag anything expiring within 30 days to Tony
- Never action directly — flag only

## Client Folder Audit (WEEKLY)
- Scan OneDrive client folders for Iconic Investors clients
- Check for required docs: SOA, FSG, LoE, ID, fact find
- Report gaps to Tony

## GitHub Sync (EVERY HEARTBEAT)
Check if MEMORY.md, TOOLS.md, USER.md or daily memory files have changed.
If so, sync to tony583/aria-memory on GitHub.

## Marketing Monitor (DAILY)
- Review GHL campaign performance (open rates, click rates, responses)
- Check if any scheduled content needs publishing or approval
- Flag new content opportunities based on pipeline activity
- Draft 1 social post per day for Tony's approval queue
- Track lead-to-client conversion rates weekly

## Email Monitoring — connect@iconicinvestors.com.au (EVERY HEARTBEAT)
Check for new emails in connect@iconicinvestors.com.au inbox.
- IMAP: imap.gmail.com:993
- Login: connect@iconicinvestors.com.au / app password in TOOLS.md
- Flag any replies from displaced advisers to Tony via Telegram immediately
- Log new emails in daily memory file
- If a reply looks like an interested adviser → update their GHL stage to "Responded"
- If nothing new → NO_REPLY

## Checks (rotate 2-4x per day)
- GHL: new leads or pipeline updates?
- Marketing: any campaigns due, content to approve?
- Compliance: anything expiring?
- Website: any content that needs updating?
- Nothing urgent → reply HEARTBEAT_OK
