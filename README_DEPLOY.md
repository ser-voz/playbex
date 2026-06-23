# Playbex - Growth Stack landing (deploy bundle)

Static, multilingual B2B landing for the Playbex Growth Stack, plus a confidential unit-economics proof page.

## Contents
- index.html ............. English (canonical https://playbex.com/)
- es/index.html .......... Spanish (https://playbex.com/es/)
- pt/index.html .......... Portuguese (https://playbex.com/pt/)
- proof.html ............. Unit-economics proof page (noindex, confidential - share via data room, not linked publicly)
- assets/ ................ logo, favicon, apple-touch, PWA icons 192/512
- og-growth-stack.png .... social share image (1200x630)
- favicon.ico ............ legacy favicon (root)
- site.webmanifest ....... PWA manifest
- robots.txt ............. allows all, blocks /proof.html, points to sitemap
- sitemap.xml ............ 3 language URLs with reciprocal hreflang
- 404.html ............... branded not-found page

## How to publish
Any static host works (Netlify, Vercel, Cloudflare Pages, GitHub Pages, S3 + CloudFront, nginx/Apache).
Deploy the folder at the SITE ROOT so paths resolve:
- https://your-domain/            -> index.html
- https://your-domain/es/         -> es/index.html
- https://your-domain/pt/         -> pt/index.html
- /favicon.ico, /site.webmanifest, /assets/* served from root.

Netlify/Vercel: drag-and-drop or connect the folder; no build step (pure static).
nginx example:
    server { root /var/www/playbex; index index.html;
             location / { try_files $uri $uri/ =404; } error_page 404 /404.html; }

## BEFORE GOING LIVE - configure 4 things
1) Base URL. If the domain is NOT https://playbex.com/, replace that string everywhere:
   in index.html, es/index.html, pt/index.html, proof.html (canonical, hreflang, og:url, og:image)
   and in robots.txt and sitemap.xml. (Cross-page language links are relative and need no change.)
2) CRM endpoint. In each HTML, find  CRM_ENDPOINT = "https://your-endpoint.example.com/api/leads"
   and set your CRM/webhook URL (own backend or serverless proxy; do NOT put secret API keys in the file).
3) Analytics. The form already pushes window.dataLayer 'lead_submit'. Add your GA4/GTM loader snippet
   in <head> (or before </body>) to receive it.
4) Terms page. Footer "Terms" link is "#". Point it to your terms URL. Privacy already links to
   https://playbex.com/privacy.html - update if different.

## Notes
- Forms submit via fetch() to the CRM endpoint, with honeypot + consent + success/error states and a
  graceful mailto fallback (contact@playbex.com) only if the request fails.
- Logo and favicons are embedded (data URI) in the HTML, so pages render even before /assets resolve.
- proof.html is noindex and disallowed in robots; keep it for the data room, not public navigation.
- Replace og-growth-stack.png with a custom 1200x630 if you have brand art.
