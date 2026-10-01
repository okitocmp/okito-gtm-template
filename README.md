# Okito CMP — Google Tag Manager Community Template

Google recommends that CMPs publish a consent mode template in the GTM
Community Template Gallery
(https://developers.google.com/tag-platform/security/concepts/cmp). This
folder is that template.

What the tag does (same behaviour as the Okito CDN banner and head snippet):

- Opt-in regions (EEA, UK, Switzerland, Türkiye, Brazil and similar; the US
  unless "US opt-out" is ticked) start **denied**, `wait_for_update` 500 ms.
- Everywhere else starts **granted**, `wait_for_update` 0, so Data
  Transmission Controls / Global Consent Defaults applied regardless of region
  do not remove measurement ("Measurement off until a choice" → denied
  everywhere).
- Global Privacy Control → denied. GTM's sandbox cannot read
  `navigator.globalPrivacyControl`, so the Okito CDN script sends the denied
  `consent update` right after it loads.
- Sets `developer_id.dZGJiMm` (Okito's Google CMP developer ID),
  `ads_data_redaction` and, optionally, `url_passthrough`.
- "My banner uses IAB TCF" (off by default) adds the IAB TCF API stub:
  `__tcfapi` is created with `createArgumentsQueue` and queues its calls in
  `window.__okitoGtmTcfQueue`, which the Okito script answers once it has
  loaded (on sites without TCF it answers them "no consent" for GDPR visitors
  and removes the stub). An existing `__tcfapi` is left alone. There is no
  `__tcfapiLocator` iframe (the sandbox cannot add one).
- "My banner shows the US State Laws notice (IAB GPP)" (off by default) adds
  the IAB Global Privacy Platform API: `__gpp` is created with
  `createArgumentsQueue` and queues its calls in `window.__okitoGtmGppQueue`,
  which the Okito script answers once it has loaded (it also adds the
  `__gppLocator` iframe). An existing `__gpp` is left alone. Leave it off
  when the banner uses IAB TCF.
- Loads `https://cdn.okito.com/js/<website key>`, which shows the banner (or
  not, per the site settings) and sends `consent update` / IAB TCF signals.

Customers add the tag with the **Consent Initialization - All Pages** trigger
and must not also paste the Okito embed script.

## Publishing a new version

The template is published from https://github.com/okitocmp/okito-gtm-template
(Gallery: `okitocmp/okito-gtm-template`). This folder is the source of truth.

1. Import `template.tpl` in a GTM workspace (Templates → New → ⋮ → Import),
   run the tests in the template editor, and try it on the review site.
2. Copy `template.tpl` to the repository root and commit.
3. In `metadata.yaml`, replace the placeholder `sha` of the newest version
   with that commit's full sha, copy the file to the repository and commit.
   The Gallery picks the new version up automatically; containers using the
   template see an update notice.
