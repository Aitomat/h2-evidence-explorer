# H2 Evidence Explorer — free embeddable widget

A free, always-current overview of human studies on **molecular hydrogen (H₂)**, ready to drop into any website with a single `<iframe>`.

> **Live:** [h2medicine.org](https://h2medicine.org) (English) · [h2medizin.org](https://h2medizin.org) (Deutsch)

---

## What it is

The **H2 Evidence Explorer** is a science-first, ad-free overview of the peer-reviewed human research on molecular hydrogen (H₂). It lets anyone browse, filter, and search the studies — by indication, delivery method, evidence level, and more.

- **Science-first.** Every entry links straight to the peer-reviewed source. Click a study and it opens the original paper on **PubMed / DOI** — no middleman, no spin.
- **Ad-free. No tracking.** No advertising, no analytics surveillance, no cookies sold. Just the evidence.
- **Always current.** The dataset is maintained centrally by H2Medicine and updates automatically. Embedders never have to touch or maintain anything.
- **Bilingual.** Search and content work in both **German and English**.
- **Not medical advice.** This is a research overview for information and education. It is not medical advice, diagnosis, or treatment. Always consult a qualified healthcare professional.

---

## Why this is open source

This repository is public on purpose.

The studies, the curation, the build pipeline, and the full site stay private — but the **embed itself is free and open** so that **anyone** can put the evidence on their own site:

- **Researchers & clinicians** can show the current human-study landscape on a department or lab page.
- **Bloggers, journalists & educators** can embed a credible, citable overview instead of cherry-picked claims.
- **Communities & nonprofits** can spread access to the underlying science.

The more places the Evidence Explorer lives, the more the topic — and the science behind it — reaches people. Open embedding is how good evidence travels.

---

## How to embed

Drop one of these snippets anywhere on your page. The widget is fully responsive; set `width` to `100%` and adjust the height to taste.

### English (h2medicine.org)

```html
<iframe
  src="https://h2medicine.org/widget/"
  title="H2 Evidence Explorer"
  width="100%"
  height="900"
  style="border:0;width:100%;max-width:100%;"
  loading="lazy"
  referrerpolicy="no-referrer-when-downgrade">
</iframe>
```

### German (h2medizin.org)

```html
<iframe
  src="https://h2medizin.org/widget/"
  title="H2 Evidenz-Explorer"
  width="100%"
  height="900"
  style="border:0;width:100%;max-width:100%;"
  loading="lazy"
  referrerpolicy="no-referrer-when-downgrade">
</iframe>
```

### Optional: responsive auto-height

By default you pick a fixed `height`. If you'd like the iframe to grow and shrink with its content, add this small script. It listens for height messages the widget posts to the parent window:

```html
<iframe id="h2ee" src="https://h2medicine.org/widget/"
        title="H2 Evidence Explorer"
        width="100%" height="900"
        style="border:0;width:100%;max-width:100%;" loading="lazy"></iframe>

<script>
  // Auto-resize the H2 Evidence Explorer iframe to its content height.
  window.addEventListener('message', function (e) {
    // Only trust messages from the official widget origins.
    if (e.origin !== 'https://h2medicine.org' && e.origin !== 'https://h2medizin.org') return;
    var data = e.data || {};
    if (data && data.h2ee && typeof data.height === 'number') {
      var f = document.getElementById('h2ee');
      if (f) f.style.height = data.height + 'px';
    }
  });
</script>
```

> If your page never receives a height message (e.g. the feature isn't available in your browser), the iframe simply keeps the fixed `height` you set — so it always works.

### URL parameters

Pre-filter the widget by appending query parameters to the `/widget/` URL. Combine as many as you like with `&`.

| Parameter     | What it does                                  | Example                         |
|---------------|-----------------------------------------------|---------------------------------|
| `?indication=`| Pre-filter by indication / condition          | `?indication=respiratory`       |
| `?method=`    | Pre-filter by delivery method                 | `?method=inhalation`            |
| `?ev=`        | Pre-filter by evidence level                  | `?ev=rct`                       |
| `?human=`     | Show only human studies (`1` = on)            | `?human=1`                      |
| `?q=`         | Pre-fill the search box (bilingual DE/EN)     | `?q=sport`                      |
| `?titlelink=` | Set to `off` to disable the title link        | `?titlelink=off`                |

**Example — human respiratory studies only:**

```
https://h2medicine.org/widget/?indication=respiratory&human=1
```

Search is bilingual: a query like `?q=Entzündung` and `?q=inflammation` both work, in either language widget.

---

## Open JSON API

The same data powering the widget is available as plain JSON. The API is **CORS-open** and needs **no authentication** — fetch it from anywhere.

| Endpoint                | Contents                                  |
|-------------------------|-------------------------------------------|
| `/api/meta.json`        | Dataset metadata (counts, last updated)   |
| `/api/catalog.json`     | Filter catalog (indications, methods, …)  |
| `/api/studies.json`     | The studies list                          |
| `/api/graph.json`       | Relationship graph data                   |

Base URLs: `https://h2medicine.org` (EN) and `https://h2medizin.org` (DE).

```js
// Example: fetch dataset metadata
const meta = await fetch('https://h2medicine.org/api/meta.json').then(r => r.json());
console.log(meta);
```

### MCP server (for AI agents)

For AI agents and assistants, an **MCP server** exposes the same evidence:

- **EN:** `https://mcp.h2medicine.org`
- **DE:** `https://mcp.h2medizin.org`

---

## Attribution / credit

Embedding is free. In return, please **keep the visible "Data: h2medicine.org" credit link** that the widget shows, and — if you consume the JSON API or MCP server — attribute the source with a link back to **h2medicine.org** (or **h2medizin.org**). It keeps the evidence traceable and helps others find the original, current dataset.

A simple line works:

```html
<p>Data: <a href="https://h2medicine.org">h2medicine.org</a></p>
```

---

## Important notices

- **Not medical advice.** The Evidence Explorer is an informational research overview. It is **not** medical advice, diagnosis, or treatment, and nothing here should replace consultation with a qualified healthcare professional.
- **Data ownership.** The study data is **© and curated by H2Medicine**. *This repository* only documents the **free embed** (iframe + open API) and provides example HTML — it does **not** contain or redistribute the underlying dataset, curation, or build pipeline.

---

## What's in this repo

| File / folder            | Purpose                                            |
|--------------------------|----------------------------------------------------|
| `README.md`              | This guide                                         |
| `LICENSE`                | MIT — covers the embed snippets & docs here        |
| `examples/iframe-en.html`| Minimal standalone English embed + auto-height     |
| `examples/iframe-de.html`| Minimal standalone German embed + auto-height      |
| `examples/mini-embed.html`| Parametrized example (pre-filtered)               |

The **MIT license applies to the snippets and documentation in this repository**, not to the H2Medicine dataset.
