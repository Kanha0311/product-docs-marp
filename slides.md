---
marp: true
theme: product-docs
paginate: true
title: Product Documentation — Widgets API
author: Technical Writing — Software Co.
---

<!-- _class: lead -->
# Widgets API — Product Documentation

### Maintainable docs, version-controlled, multi-format

**Contact:** 23f3003124@ds.study.iitm.ac.in

<!-- _footer: _v1.0 • Built with Marp_ -->

---

## About this Deck

- Written entirely in **Markdown**
- Export to **HTML/PDF/PPTX/PNG** with `marp-cli`
- Ready for CI builds & **version control**
- Uses a **custom theme** and **Marp directives**

> Tip: Keep content modular (per feature) and reuse partials.

---

![bg cover](images/hero.jpg)

# Widgets API Overview

- REST + Webhook callbacks
- JSON over HTTPS
- Auth: OAuth2 client credentials

<!-- _color: white -->
<!-- _header: **Widgets API** -->
<!-- _footer: _Docs © YourCo_ -->

---

## Quickstart

1. Obtain client credentials  
2. Create a widget  
3. Render in your app  

```bash
curl -X POST https://api.example.com/v1/widgets \
  -H "Authorization: Bearer $TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"name":"Pricing Table","variant":"pro"}'
Data Model
Field	Type	Notes
id	string	ULID
name	string	1–80 chars
variant	enum	basic | pro | enterprise
createdAt	datetime	ISO 8601

<!-- _footer: _IDs are immutable_ -->
Pagination & Complexity
Inline math: Average page cost is $O(1)$ with tokens.

Block math for end-to-end processing time:

𝑇
(
𝑛
)
=
𝑇
𝑛
𝑒
𝑡
(
𝑛
)
+
𝑇
𝑐
𝑝
𝑢
(
𝑛
)
=
𝑂
(
𝑛
log
⁡
𝑛
)
+
𝑂
(
𝑛
)
=
𝑂
(
𝑛
log
⁡
𝑛
)
T(n)=T 
net
​
 (n)+T 
cpu
​
 (n)=O(nlogn)+O(n)=O(nlogn)
Where $n$ is objects scanned; ensure server timeouts use $T_{SLA} > c,n\log n$.

Code Samples (TypeScript)
ts
Copy
Edit
import fetch from "node-fetch";

export async function createWidget(token: string, body: any) {
  const res = await fetch("https://api.example.com/v1/widgets", {
    method: "POST",
    headers: { "Authorization": `Bearer ${token}`, "Content-Type": "application/json" },
    body: JSON.stringify(body)
  });
  if (!res.ok) throw new Error(await res.text());
  return res.json();
}
<!-- _footer: _Example only; handle retries & idempotency_ -->
<!-- _backgroundColor: #0b132b --> <!-- _color: #e0e1dd -->
Webhooks
Configure an HTTPS endpoint

Verify X-Signature

Retry with exponential backoff

json
Copy
Edit
{
  "event": "widget.updated",
  "id": "01J9Y7AX…",
  "signature": "v1,5d2c…",
  "data": { "id": "01J9…", "name": "Pricing Table" }
}
Versioning Strategy
Semantic: MAJOR.MINOR.PATCH

Deprecations announced ≥ 90 days

Changelogs per release

<!-- _header: **Governance** -->
Export Commands
bash
Copy
Edit
# HTML for the web
marp slides.md -o dist/slides.html

# PDF for sharing (allow local images)
marp slides.md --pdf --allow-local-files -o dist/WidgetsAPI.pdf

# PPTX for stakeholders
marp slides.md --pptx -o dist/WidgetsAPI.pptx
Thank You
Questions? Feedback?

Email: 23f3003124@ds.study.iitm.ac.in
