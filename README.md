# Uber SendGrid Dangling CNAME Takeover PoC  
**Vulnerable subdomain:** 15205598.uber.com  
**Issue:** Dangling CNAME pointing to sendgrid.net → possible Inbound Parse takeover  
**Impact:** Potential interception of all emails sent to @15205598.uber.com  
**Status:** In scope (confirmed in current public asset list)  
**Date of discovery:** February 22, 2026  
**Researcher:** Neha (@hackerbughunter.online)

## Summary
A dangling CNAME record allows anyone with a SendGrid account to potentially claim the subdomain via Inbound Parse and receive parsed copies of all inbound emails.

## Evidence (all collected February 22, 2026)

### 1. Scope confirmation
Asset is listed in Uber's public bug bounty domain list:  
https://appsec-analysis.uber.com/public/bugbounty/ListDomains

Entry:
```json
{"Name":"15205598.uber.com","DNSProvider":"ultradns"}
→ images/scope-proof-fresh-20260222.png
2. Dangling CNAME proof
Bashdig 15205598.uber.com CNAME
→ 15205598.uber.com. 600 IN CNAME sendgrid.net.
Full trace: images/trace-cname-excerpt.png
3. SendGrid Inbound Parse attempt
Tried adding real subdomain → verification blocked (expected).
→ images/inbound-parse-setup.png
→ images/verification-blocked.png
4. Routing proof (real subdomain)
Email sent to test@15205598.uber.com → routed to SendGrid MX → bounced 550
→ images/sendgrid-bounce-fresh-20260222.png
5. Full interception proof (controlled domain)
Configured: 15205598.uber.com.hackerbughunter.online → webhook.site
Email delivered → parsed → POSTed full content + delivered event
→ images/webhook-parsed-content-20260222.png
→ images/webhook-delivered-event-20260222.png
Files in this repo

README.md
images/ (all screenshots)
uber_dns_fresh_*.txt
uber_dns_trace_fresh_*.txt
trace_cname_excerpt.txt
swaks_target_final_*.txt
scope_json_*.json

Legal / Ethical note
This is a proof-of-concept for security research purposes only. No unauthorized access or exploitation was performed. All testing was done on researcher-controlled domains and followed responsible disclosure guidelines.
References

Similar historical Uber SendGrid cases
SendGrid Inbound Parse documentation: https://docs.sendgrid.com/for-developers/parsing-email/setting-up-the-inbound-parse-webhook

