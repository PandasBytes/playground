# NL-to-SQL Eval Framework

An interactive visual reference for how evaluation works in LLM-powered NL-to-SQL systems — from initial query to production feedback loop.

**Who this is for:** PMs, ML engineers, and data practitioners building or buying AI products where a natural-language interface sits on top of a structured data store. If you've ever shipped a "chat with your data" feature and wondered how to know when it's actually working — this is for you.

---

## What's inside

Eight tabbed diagrams, each covering a distinct layer of the eval system:

| Tab | What it covers |
|-----|----------------|
| LLM software overview | The full stack: what you're building and where eval fits |
| The flywheel | How offline evals, online signals, and human review compound over time |
| Overview — two loops | The offline/online loop structure at a glance |
| Offline eval flow | Test suite → LLM judge → golden dataset → regression gate |
| Online eval flow | Async scoring of live queries; zero latency impact on users |
| RAG & retrieval | Schema retrieval, few-shot selection, and how retrieval quality affects SQL correctness |
| Failures | Taxonomy of failure modes: ambiguous intent, wrong schema, hallucinated columns, stale gold labels |
| References | Source material and further reading |

---

## View it live

[Open the interactive diagram](https://pandasbytes.github.io/playground/nl_to_sql_eval_framework/) *(GitHub Pages)*

Or clone and open `index.html` locally — no build step, no dependencies.

---

**Emeri Z.** · [LinkedIn](https://www.linkedin.com/in/emeri-z/)
