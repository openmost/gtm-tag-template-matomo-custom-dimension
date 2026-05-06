# Matomo Custom Dimension — Google Tag Manager template

A Google Tag Manager community template that sets a **custom dimension** on the current Matomo visit or page, through the standard `_paq` queue.

Authored by Ronan HELLO — [Openmost](https://openmost.com).

---

## What this tag does

When the tag fires, it pushes a `setCustomDimension` call to the Matomo tracker:

```js
_paq.push(['setCustomDimension', customDimensionId, customDimensionValue]);
```

The dimension value is then attached to the **next** tracking call (`trackPageView`, `trackEvent`, etc.) sent by Matomo on the same page. This is the recommended pattern for *Visit*- and *Action*-scoped dimensions.

Typical use cases:

- Logged-in vs. anonymous user
- User role / plan / segment
- Article author, category, language
- A/B test variant
- Any contextual attribute you want to slice your Matomo reports by

---

## Prerequisites

1. A working **Matomo** instance (self-hosted or Matomo Cloud).
2. A **Google Tag Manager** container loaded on your site.
3. The Matomo base tracker (`_paq`) must already be initialised on the page.
4. A **custom dimension** must already be configured in Matomo (*Administration → Websites → Custom Dimensions*) — note its **ID** and **scope**.

This template only **adds the dimension to the `_paq` queue**; it does not load the Matomo tracker itself.

---

## Installation

### Recommended — install from the Community Template Gallery

This is the easiest path and gives you automatic update notifications when a new version is published.

1. In GTM, open your workspace and go to **Templates → Tag Templates → Search Gallery**.
2. Search for **"Matomo Custom Dimension"** by **Openmost**.
3. Click **Add to workspace** and accept the requested permissions.

That's it — the tag type **Matomo Custom Dimension** is now available when you create a new tag.

### Alternative — import `template.tpl` manually

Use this only if you can't access the Community Gallery (e.g. private GTM environment) or want to fork / customise the template.

1. In GTM, go to **Templates → Tag Templates → New**.
2. Open the menu (⋮) → **Import**.
3. Select `template.tpl` from this repository.
4. Save.

---

## Configuration

Once the template is added, create a new tag of type **Matomo Custom Dimension** and fill in the fields below.

| Field | Required | Description |
|---|---|---|
| **Custom dimension id** | Yes | The numeric ID of the custom dimension as configured in Matomo (e.g. `1`, `2`, `3`…). |
| **Custom dimension value** | Yes | The value to record. Usually a GTM variable — e.g. `{{User Role}}`, `{{DLV - article.category}}`. |

### Tip — using GTM variables

The value field accepts any GTM variable, so the dimension can be built dynamically from the dataLayer, cookies, URL parameters, etc.

---

## Triggering

Where you place this tag depends on the **scope** of your custom dimension:

- **Visit-scoped dimension** — fire the tag on `All Pages` (or as early as possible during the visit), **before** the Matomo pageview. Use Tag Sequencing if needed.
- **Action-scoped dimension** — fire the tag on the same trigger as the action it should be attached to (e.g. just before a Matomo Event tag), so the dimension is set before the action is sent.

> **Important:** Matomo applies the dimension to the **next** tracking call. If no tracking call follows on the same page, the dimension will not be recorded.

---

## Permissions

The template requests one permission:

- **Access global variables → `_paq`** (read / write / execute) — required to push the dimension into Matomo's tracker queue.

No external network requests are made directly by the template.

---

## Troubleshooting

- **Dimension not visible in Matomo reports** — confirm the dimension is **active** in Matomo's admin, and that its scope matches how you fire the tag.
- **`_paq is not defined`** — the Matomo base tracker hasn't loaded yet. Adjust your trigger or use Tag Sequencing so the Matomo tag fires first.
- **Wrong value recorded** — check the GTM Preview mode → *Variables* tab to confirm the variable resolves to the expected value at firing time.

---

## License

See [LICENSE](LICENSE).
