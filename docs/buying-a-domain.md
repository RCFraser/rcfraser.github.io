# Buying a custom domain for this website

You started with the GitHub-generated URL (`rcfraser.github.io`), which works fine on its own. This file documents how to upgrade to a custom domain like `ryan-c-fraser.com` if you decide to later — common move before going on the job market.

Nothing about starting on `rcfraser.github.io` makes this harder later. GitHub automatically redirects the old URL to the new one, so any links already shared keep working.

## Recap of available names (as of 2026-05-20)

- `ryan-c-fraser.com` — available for normal registration (~$10/yr)
- `ryanfraser.com` — registered since 2000, parked, aftermarket-only (expensive)
- `rcfraser.com` — registered since 2007, parked, aftermarket-only (expensive)
- `ryanfraser.io`, `ryanfraser.me`, `rcfraser.io` — DNS suggests likely available; re-confirm with WHOIS before buying

Recommended pick: **`ryan-c-fraser.com`**. The hyphens are a minor verbal cost; the `.com` is worth it. Going aftermarket for unhyphenated versions costs hundreds-to-thousands and isn't worth it at the grad-student stage.

## Where to buy

A "domain registrar" is a company licensed to sell domain names. You pay them annually to register the name to you.

| Registrar | Cost for `.com`/yr | Notes |
|---|---|---|
| **Cloudflare Registrar** | ~$10.44 | At-cost pricing, no upsells, free WHOIS privacy. Recommended. |
| Namecheap | ~$11–13 | Friendlier UI. Decline all add-ons (privacy, "premium DNS", SSL) at checkout — they're either free elsewhere or unnecessary. |
| GoDaddy | ~$20+ | Avoid. Aggressive upsells, opaque renewal pricing. |

## Steps to register at Cloudflare

1. Sign up for a free account at https://dash.cloudflare.com/sign-up
2. In the dashboard, click **Domain Registration** → **Register Domains**
3. Search for the name you want
4. Add to cart, decline any add-ons, pay
5. Confirm WHOIS privacy is on (it's free and on by default at Cloudflare — your home address won't be public)
6. Set **auto-renew on**. Forgetting to renew is the #1 way people lose domains they care about.

## Connecting the domain to GitHub Pages

Once the domain is yours, two things need to happen: DNS configuration (at Cloudflare) and repo configuration (here).

Authoritative reference: [GitHub's docs on custom domains](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site/managing-a-custom-domain-for-your-github-pages-site). The instructions below match those at time of writing; if GitHub's IPs change in the future, theirs are the source of truth.

### 1. Configure DNS at Cloudflare

In Cloudflare dashboard → your domain → **DNS** → **Records** → add:

| Type | Name | Content | Proxy status |
|---|---|---|---|
| A | @ | 185.199.108.153 | DNS only (gray cloud) |
| A | @ | 185.199.109.153 | DNS only |
| A | @ | 185.199.110.153 | DNS only |
| A | @ | 185.199.111.153 | DNS only |
| CNAME | www | rcfraser.github.io | DNS only |

The four A records point your apex domain (`ryan-c-fraser.com`) at GitHub Pages servers. The CNAME makes `www.ryan-c-fraser.com` also work.

**Important**: set the orange proxy cloud to **OFF** (DNS only). Cloudflare's proxy interferes with GitHub Pages' HTTPS certificate provisioning. Cloudflare will warn you about this — ignore the warning, GitHub handles HTTPS itself.

### 2. Configure the repo

Add a file at the repo root called `CNAME` (no extension) containing one line:

```
ryan-c-fraser.com
```

Commit and push. GitHub will detect it.

### 3. Enable HTTPS in GitHub Pages

GitHub repo → **Settings** → **Pages** → check **Enforce HTTPS**. The checkbox may be disabled for the first few minutes while GitHub issues the certificate. If it's still disabled after an hour, uncheck and recheck the custom domain field to retrigger.

### 4. Wait

DNS propagation typically takes 5 minutes but can take up to 24 hours. Once it's done, `ryan-c-fraser.com` will load your site, and `rcfraser.github.io` will redirect to it.

## Recurring cost

~$10/yr forever. Set a calendar reminder for renewal or rely on auto-renew.
