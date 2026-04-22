# Infrastructure

## How a request gets served

```
 Browser
   ↓ Cloudflare edge
   ├─ TLS termination  (A+ cipher suite, TLS 1.2/1.3 only)
   ├─ WAF + Bot Fight Mode + rate limiting
   ├─ Response header injection  (HSTS, CSP, COEP, CORP …)
   ├─ CDN cache  (long-lived for /_astro/* immutable assets)
   └─ HTTP/3 (QUIC) support
   ↓
 Origin nginx on Ubuntu
   ├─ Port 80 → 301 redirect to HTTPS
   ├─ Port 443 TLS 1.2/1.3
   └─ try_files for the static files
   ↓
 Static site
   └─ /var/www/html/cbsworcester.com/   (output of `npm run build`)
```

## Why static

Static site generation takes a lot of problems off the table:

- No server-side code means no runtime to exploit
- No database means nothing to inject into
- No login means no credentials to steal
- Every deploy is a fresh, identical build

The only dynamic piece is the contact form, and that runs on Formspree so there's no backend code to maintain. If the form ever has a bad day, the rest of the site keeps working.

## Getting the most out of Cloudflare

Cloudflare isn't hosting the site — the origin is a small Ubuntu VPS I run — but it's in front of it doing a lot of work that would otherwise fall to nginx. Leaning into that is a deliberate choice, and it's the reason a family-business site on a single VPS can carry the grades it does without expensive infrastructure.

Here's what Cloudflare actually handles:

| Layer | What Cloudflare does | Why that matters |
|---|---|---|
| **DNS** | Authoritative DNS for cbsworcester.com | Fast global resolution, propagation in seconds |
| **Proxy** | All traffic flows through Cloudflare's global network of edge servers before touching the origin | The origin IP is never exposed publicly, which hides it from basic DDoS and scanning attempts |
| **TLS** | Termination with a modern cipher suite, HTTP/2, HTTP/3 (QUIC) | A+ on SSL Labs without hand-tuning nginx |
| **Headers** | Transform Rules inject HSTS, CSP, X-Frame-Options, Referrer-Policy, Permissions-Policy, COEP / COOP / CORP on every response | A on SecurityHeaders, policy managed in one place |
| **WAF** | Managed rules + Bot Fight Mode filter automated abuse | Scripted scanners and credential-stuffing never reach the origin |
| **Cache** | Long-lived caching for immutable `/_astro/*` assets | Origin load is close to zero; global latency is a few ms |
| **Email reports** | DMARC aggregation service collects receiver reports | I can actually read what's happening with outbound mail without parsing raw DMARC XML |

Because Cloudflare is doing all of the above, the origin nginx config is deliberately minimal — no security headers, no custom compression tuning, no fastcgi, nothing clever. Its entire job is to redirect HTTP to HTTPS and serve static files off disk. Simpler configs mean fewer things to misconfigure.

## About the multi-tenant host

The VPS serving cbsworcester.com also runs a [T-Pot](https://github.com/telekom-security/tpotce) honeypot — a research environment where I collect real attacker behavior in a controlled way. That might sound alarming next to a family business's website. It's intentional, and it's isolated.

- T-Pot runs entirely inside Docker, with its **own nginx** fronting its services inside the container stack.
- The production sites (cbsworcester.com and a handful of others) are served by the **host nginx** at `/etc/nginx/`.
- The two don't share ports, network namespaces, or config files.
- Administrative access uses different SSH ports.

If an attacker ever got into the honeypot, they'd have to break out of Docker *and* pivot across network boundaries to touch anything production. That's not trivial, and it's something I actively monitor.

The full honeypot write-up lives in my [IT / Security portfolio](https://github.com/TarekAloch/tarek-portfolio) on GitHub, alongside the rest of my security work.

## Deploying

```
Developer laptop
   ↓  npm run build
./dist/   (static HTML + assets, ~4 MB)
   ↓  rsync -az --delete dist/ ubuntu:/var/www/html/cbsworcester.com/
Origin nginx serves the new build instantly
   ↓
Cloudflare cache picks it up on next fetch
```

Deploying is just copying files. No services to restart, no database to migrate, nothing to stage or babysit during rollout. If something ever broke, I'd just rsync again — that's the whole rollback story.
