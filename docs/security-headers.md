# HTTP security headers

## D → A

When I took the site over, [SecurityHeaders by Snyk](https://securityheaders.com/?q=cbsworcester.com) gave it a **D**. After turning on the right Cloudflare response-header rules, it's an **A**.

![SecurityHeaders: D before, A after](./portfolio/securityheaders-before-after.jpeg)

## What the site actually returns

From `curl -I https://cbsworcester.com/`:

| Header | Value | What it does |
|---|---|---|
| `strict-transport-security` | `max-age=15552000` | Forces HTTPS for 180 days |
| `content-security-policy` | (configured at Cloudflare) | Restricts what the browser will load — primary XSS defense |
| `x-content-type-options` | `nosniff` | Stops browsers from guessing content types |
| `x-frame-options` | `SAMEORIGIN` | Blocks clickjacking via iframes |
| `referrer-policy` | `strict-origin-when-cross-origin` | Doesn't leak full URLs across origins |
| `permissions-policy` | `accelerometer=(), camera=(), geolocation=(), gyroscope=(), magnetometer=(), microphone=(), payment=(), usb=()` | Turns off hardware access the site doesn't need |
| `cross-origin-embedder-policy` | `require-corp` | Enables cross-origin isolation |
| `cross-origin-opener-policy` | `same-origin` | Isolates from the opener window |
| `cross-origin-resource-policy` | `same-origin` | Restricts cross-origin embedding |

The last three (COEP / COOP / CORP) are a bonus. They enable browser-level cross-origin isolation, which mitigates Spectre-class attacks and isn't required for an A rating. Most sites don't bother with them. I figured, why not.

## Why the headers live at Cloudflare

The origin nginx has zero `add_header` directives. That's on purpose.

Putting the rules at the edge means:

- Policy changes deploy in seconds through the Cloudflare dashboard. No nginx reload. No deploy pipeline.
- Every site on the Cloudflare account can share a baseline via Transform Rules.
- The origin is simpler and easier to reason about — if something weird is happening with the headers, I know it's not nginx.

More on that choice in [infrastructure.md](./infrastructure.md).

## Verify it yourself

```bash
curl -sSI https://cbsworcester.com/ \
  | grep -Ei 'strict-transport|content-security|frame-options|content-type-options|referrer|permissions|cross-origin'
```

Or paste the domain into [securityheaders.com](https://securityheaders.com/?q=cbsworcester.com).
