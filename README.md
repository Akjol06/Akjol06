<div align="center">

<h1><samp>A&nbsp;K&nbsp;J&nbsp;O&nbsp;L&nbsp;&nbsp;&nbsp;K&nbsp;A&nbsp;N&nbsp;A&nbsp;E&nbsp;V</samp></h1>

<p><samp>full-stack engineer &nbsp;·&nbsp; i build AI agents that answer real customers</samp></p>

<p>
<a href="https://aivo.kg"><samp>aivo.kg</samp></a> &nbsp;·&nbsp;
<a href="https://t.me/kanaev006"><samp>telegram</samp></a> &nbsp;·&nbsp;
<a href="mailto:aivo.company.info@gmail.com"><samp>email</samp></a>
</p>

</div>

```console
akjol@bishkek:~$ whoami
────────────────────────────────────────────────────────────────────
role       full-stack engineer — schema, API, UI, deploy. all of it.
building   aivo.kg · multi-tenant AI-agent platform · solo, 1 year
stack      Python 3.13 · FastAPI · React 19 · Postgres · Redis
timezone   UTC+6 · Bishkek, KG · replies same day
status     open — freelance & product work
```

## <samp>01 &nbsp;· &nbsp;what I actually do</samp>

I don't hand off half a system. I take a product from an empty database to a
domain someone can type into a browser — and then I keep it running.

```
▸  AI agents          LLM + RAG that hold a real conversation, capture the
                      order, and write it where the business already looks.

▸  Clean backends     domain-driven, strict tenant isolation, idempotent money,
                      heavy work pushed off the request path.

▸  UI that behaves    real-time chat, responsive layouts, motion that means
                      something instead of decorating.

▸  Ownership          first commit to production incident. No "that's frontend's
                      problem."
```

## <samp>02 &nbsp;· &nbsp;aivo — system map</samp>

> A SaaS where a company builds a no-code AI agent, connects it to their
> channels, and lets it handle conversations, orders and data sync — 24/7.

```
 ┌─ CHANNELS ─────────┐   ┌─ CORE ──────────────────────┐   ┌─ DATA ──────────────┐
 │                    │   │                             │   │                     │
 │  Telegram          │   │   FastAPI (async ASGI)      │   │  PostgreSQL         │
 │  WhatsApp          │──▶│   ↓                         │──▶│  + pgvector         │
 │  Instagram         │   │   agent runtime             │   │  + tsvector / trgm  │
 │  Web widget        │   │   ↓                         │   │                     │
 │  (Shadow DOM)      │◀──│   tool calls · RAG · memory │◀──│  Redis · ARQ queue  │
 │                    │   │                             │   │                     │
 └────────────────────┘   └──────────────┬──────────────┘   └─────────────────────┘
                                         │
                          ┌──────────────┴──────────────┐
                          │  Sheets · Calendar · OAuth  │
                          │  Bitrix24 · amoCRM · billing│
                          └─────────────────────────────┘

 models   OpenAI · Claude · Gemini · Groq · DeepSeek   (swappable per tenant)
 infra    Docker · Nginx · Cloudflare · VPS · CI dependency audits
 live     https://aivo.kg
```

## <samp>03 &nbsp;· &nbsp;decisions I'd defend in review</samp>

<details>
<summary><samp><b>Why hybrid retrieval instead of pure vector search</b></samp></summary>

<br/>

Pure embeddings quietly fail on the two things our users type most: three-word
queries and product codes. So retrieval runs three ways in parallel — dense
vectors (pgvector), full-text, and trigram fuzzy — and merges them with
**Reciprocal Rank Fusion**.

RRF needs no score normalisation between wildly different scales, and it degrades
gracefully: if one channel returns garbage, the other two still rank the right
chunk first. Typos and mixed ru/ky/en text stopped being a support ticket.

</details>

<details>
<summary><samp><b>Multi-tenant isolation that doesn't rely on remembering</b></samp></summary>

<br/>

Every tenant's data lives in shared tables, which means one forgotten `WHERE`
is a data breach. So the tenant scope isn't a convention — it's enforced at the
repository layer. No query object leaves the data layer without a tenant bound
to it, and there is no "raw session" escape hatch in application code.

Cheaper than per-tenant schemas, and it survives a new developer on day one.

</details>

<details>
<summary><samp><b>Billing you can safely retry</b></samp></summary>

<br/>

Payment providers retry webhooks. Networks time out mid-confirm. Users double-tap.
So every billing transition is keyed by an idempotency key and written in one
transaction with the subscription state — replaying the same webhook ten times
produces exactly one charge and one activation.

The rule: money operations are pure functions of their key, never of arrival order.

</details>

<details>
<summary><samp><b>The widget lives in a Shadow DOM</b></samp></summary>

<br/>

The chat widget gets embedded on sites I will never see — Bitrix templates,
WordPress themes, a 2014 hand-rolled CSS file. Shadow DOM means their global
`* { box-sizing }` and their `z-index: 999999` header can't reach in, and my
styles can't leak out and break their checkout.

One `<script>` tag, no iframe scroll-height gymnastics, no CSS reset war.

</details>

<details>
<summary><samp><b>Anything slow goes to a worker</b></samp></summary>

<br/>

Document ingestion, embedding, re-indexing and CRM sync never run inside an HTTP
request. They go to ARQ on Redis, so a 40-page PDF upload returns instantly and
one heavy tenant can't starve the API for everyone else.

Rule of thumb: if it can take longer than a user's patience, it doesn't belong
in the request path.

</details>

## <samp>04 &nbsp;· &nbsp;stack</samp>

| | |
|:--|:--|
| `language` | Python 3.13 · TypeScript · PHP |
| `backend`&nbsp;&nbsp; | FastAPI · async SQLAlchemy 2 · Alembic · ARQ · Symfony |
| `frontend` | React 19 · Vite · Tailwind v4 · TanStack Query · Zustand · Framer Motion |
| `data` | PostgreSQL · pgvector · Redis · MySQL |
| `ai` | OpenAI · Claude · Gemini · Groq · DeepSeek · RAG · RRF |
| `infra` | Docker · Nginx · Cloudflare · Linux · VPS · Git |
| `tools` | Vite · Figma · Postman · PyCharm · WebStorm · VS Code |

## <samp>05 &nbsp;· &nbsp;get in touch</samp>

I'm good to work with on things that need to actually ship: an AI agent for your
sales flow, a backend that stops falling over, or a product from zero.

```console
akjol@bishkek:~$ contact --all
────────────────────────────────────────────────────────────────────
telegram   t.me/kanaev006
email      aivo.company.info@gmail.com
web        aivo.kg
open to    freelance · product work · contract
```

<p>
<a href="https://t.me/kanaev006"><img src="https://img.shields.io/badge/telegram-@kanaev006-0d1117?style=flat-square&labelColor=0d1117&color=5B45EE" alt="telegram"/></a>
<a href="mailto:aivo.company.info@gmail.com"><img src="https://img.shields.io/badge/email-say_hi-0d1117?style=flat-square&labelColor=0d1117&color=5B45EE" alt="email"/></a>
<a href="https://aivo.kg"><img src="https://img.shields.io/badge/live-aivo.kg-0d1117?style=flat-square&labelColor=0d1117&color=5B45EE" alt="aivo.kg"/></a>
</p>

<br/>

<details>
<summary><samp>the obligatory GitHub cards</samp></summary>

<br/>

<div align="center">

<img width="49%" src="https://github-profile-summary-cards.vercel.app/api/cards/stats?username=Akjol06&theme=github_dark" alt="stats"/>
<img width="49%" src="https://github-profile-summary-cards.vercel.app/api/cards/repos-per-language?username=Akjol06&theme=github_dark" alt="top languages"/>

<img width="98%" src="https://github-readme-activity-graph.vercel.app/graph?username=Akjol06&theme=github-compact&hide_border=true&area=true&color=5B45EE&line=5B45EE&point=ffffff&bg_color=0d1117" alt="activity graph"/>

</div>

</details>

<br/>

<samp><sub>built things > listed things</sub></samp>
