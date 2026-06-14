# Okito CMP — Google Tag Manager Template

Use [Okito](https://okito.com) with Google Tag Manager.

This template loads the Okito consent banner and sets the **Google Consent Mode v2**
default state (everything denied except functionality and security) before your other
tags fire. When a visitor makes a choice on the Okito banner, Okito updates consent so
your Google tags (GA4, Google Ads, etc.) respect it.

## What it does

1. Sets the Consent Mode v2 default state (privacy-first: all denied except
   `functionality_storage` and `security_storage`), with a configurable
   `wait_for_update` (default 500 ms).
2. Sets `ads_data_redaction` and `url_passthrough` (both on by default).
3. Loads the Okito CDN script: `https://cdn.okito.com/js/{websiteKey}`.

Okito's script renders the banner and, on a visitor's choice, calls
`gtag('consent', 'update', ...)`, which Tag Manager consumes.

## Setup

1. In Google Tag Manager, add the **Okito CMP** tag.
2. Enter your **Website Key** (from your dashboard at
   [app.okito.com](https://app.okito.com)). It looks like `okito-XXXXXX-XXXXXX-d`.
3. Set the trigger to **Consent Initialization - All Pages** so it runs before
   other tags.
4. (Optional) Open **Advanced settings** to change `wait_for_update`, ad data
   redaction, URL passthrough, or to add a Google Developer ID.
5. Save and publish your container.

## Fields

| Field | Required | Default | Description |
|-------|----------|---------|-------------|
| Website Key | Yes | — | Your Okito Website Key (`okito-XXXXXX-XXXXXX-d`). |
| Wait for update | No | 500 | Milliseconds to wait for a consent update. |
| Redact ads data | No | On | Remove ad identifiers while advertising consent is denied. |
| Pass ad click info through URLs | No | On | Keep identifiers (e.g. gclid) in internal links pre-consent. |
| Google Developer ID | No | dZGJiMm | Okito's Consent Mode attribution ID. |

## Links

- Website: https://okito.com
- Dashboard: https://app.okito.com
- Support: support@okito.com

## License

[Apache License 2.0](./LICENSE)
