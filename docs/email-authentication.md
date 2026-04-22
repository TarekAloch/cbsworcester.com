# Email authentication: SPF + DKIM + DMARC

Three DNS records, each doing a different job, working together so the domain can't be easily impersonated and legitimate mail actually lands in inboxes. All three are live on `cbsworcester.com`.

## SPF — who's allowed to send

**Record:** `cbsworcester.com  IN  TXT`

```
v=spf1 include:zohomail.com ~all
```

Translates to: *"Only Zoho Mail's servers can send mail as cbsworcester.com. Anyone else gets a soft fail — receivers are encouraged to treat those messages as suspicious, not to reject them outright."*

**Verify:** `dig +short TXT cbsworcester.com`

## DKIM — cryptographic signatures on every outbound message

**Record:** `zmail._domainkey.cbsworcester.com  IN  TXT`

```
v=DKIM1; k=rsa; p=MIGfMA0GCSqGSIb3DQEBAQUAA4GNADCBiQKBgQCBMvu2ec+FQg9DYKgm15IXGoAHXv8pkYDThmQIj5kC+CkHdJ91pW6jQAZ+rp6c8+RVpd5uYxorSAETzSuOSO4rZ3ObVwrwesJnDpDjG/hlKnL3uB58nU+Qw0nKQDAX+JMFoOlldE9FRhdpMBS5PTE1vTyZtlx2rmmhf0G+x7HoZQIDAQAB
```

That's Zoho's public key under the `zmail` selector. The matching private key stays at Zoho; every outbound message gets signed with it. Receivers can check the signature using the public key above — if the message was tampered with in transit, the signature breaks and the mail fails DKIM.

**Verify:** `dig +short TXT zmail._domainkey.cbsworcester.com`

## DMARC — what to do when SPF or DKIM fails

**Record:** `_dmarc.cbsworcester.com  IN  TXT`

```
v=DMARC1; p=none; rua=mailto:426491491de14f7eac108e2384f2fb71@dmarc-reports.cloudflare.net
```

- `p=none` — no enforcement yet. Receivers still check, and they still send reports, but they don't block or quarantine anything.
- `rua=...` — aggregate reports go to Cloudflare's DMARC collector, which turns raw DMARC XML into something readable.

**Why start at `p=none`:** jumping straight to `p=reject` is the classic way to accidentally break every legitimate newsletter, CRM integration, and third-party service that sends on behalf of your domain without telling you. The right rollout is `p=none` → watch the reports for a few weeks → `p=quarantine` → watch some more → `p=reject`. Cloudflare's DMARC service makes the "watching the reports" part actually pleasant.

**Verify:** `dig +short TXT _dmarc.cbsworcester.com`

## All three together

Any one of these on its own is trivially bypassed:

- **SPF alone?** Forward a message once and the SPF check breaks.
- **DKIM alone?** No one's enforcing failures.
- **DMARC alone?** Nothing to align against.

Together, they're the modern baseline. Gmail and Microsoft both require SPF + DKIM + DMARC for reliable inbox placement, and for anti-phishing enforcement to even kick in.

## References

- [M3AAWG Email Authentication Recommended Best Practices](https://www.m3aawg.org/sites/default/files/m3aawg-email-authentication-recommended-best-practices-09-2020.pdf)
- [RFC 7208 — SPF](https://datatracker.ietf.org/doc/html/rfc7208)
- [RFC 6376 — DKIM](https://datatracker.ietf.org/doc/html/rfc6376)
- [RFC 7489 — DMARC](https://datatracker.ietf.org/doc/html/rfc7489)
