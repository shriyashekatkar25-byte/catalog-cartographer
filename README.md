# Catalog Cartographer

**Which data-governance feature should ship next, when every RICE input is a guess?**
A Monte Carlo decision prototype from the APM Lab series.

- **Live demo:** https://shriyashekatkar25-byte.github.io/catalog-cartographer/
- **Gallery:** https://shriyashekatkar25-byte.github.io/apm-lab/

## The launch that inspired it

In October 2026 Databricks shipped a cluster of Unity Catalog governance and discovery updates, including **governed tag propagation via lineage (Beta, Oct 12)**, the **Discover page and domains (GA, Oct 8)**, the **Data Classification API (GA)** and **cross-engine ABAC (GA, Oct 1)**. Source: [Databricks platform release notes, October 2026](https://docs.databricks.com/aws/en/release-notes/product/2026/october).

A wave like that leaves the PM with a hard follow-up question: *with one team next quarter, which governance capability do we take to the next level?*

## The PM decision

I play the PM for catalog governance choosing among five candidates (all hypothetical next steps):

| | Candidate | What it does |
|---|---|---|
| A | Tag propagation → GA | Derived tables inherit governed tags (PII/PHI) along lineage |
| B | Auto-mask from classification | Classification results become suggested column masks |
| C | Certified-asset trust badges | Domain owners certify tables in Discover |
| D | Metadata access requests | One-click data access requests routed to owners |
| E | Cross-engine policy audit view | Which external engines read masked data, under which policy |

- **Customer problem:** tags stop at the first copy. Derived tables start life untagged, so governance leaks downstream.
- **Success metric:** share of sensitive derived tables carrying the correct governed tag within 24h, 41% → 80% (simulated baseline).
- **Guardrails:** wrong-tag rate < 0.5%, lineage-compute overhead < 2% of serverless bill, steward override rate < 5%, p95 latency on tagged tables +< 50 ms.
- **Decision rule (set before the sim):** commit only if the leader ranks #1 in ≥ 60% of simulated futures (lower 95% bound); otherwise scope or measure first.

## What the prototype shows

- **Customer problem framing:** starts from a steward's real failure mode, not a feature list.
- **Prototyping:** a working simulator with sliders for input uncertainty, sample size, the frontrunner's effort estimate and the commit bar.
- **SQL / Python:** illustrative system-table SQL to measure Reach, and a NumPy version of the simulation.
- **Data-driven decisions:** turns a two-decimal RICE ranking into "how often does this win?", with a standard error.
- **Clear writing:** a director memo and one-sentence VP summary that rewrite themselves from the live results.

At default settings (seed 102, 40% fog) tag propagation leads but wins only ~49% of futures, so the memo says *measure before betting a quarter*. Cut its effort estimate to 14 weeks and it clears the bar; push it to 26 and trust badges take over. The answer is fragile to effort, which tells the PM exactly which estimate to firm up.

## Methods (Stats 102C, explained in my own words)

| Method | Course chapter | Where it's used |
|---|---|---|
| Seeded pseudorandom uniforms | Ch. 1 | Every RICE input is drawn uniformly within its range, reproducibly |
| Probability as a fraction of hits | Ch. 1 | P(feature ranks #1) = share of futures it wins; count is Binomial |
| Standard error and SLLN | Ch. 1 | ±1.96·√(p(1−p)/n) band; running estimate settles as n grows |
| Expectation as a sample mean | Ch. 1 | Mean RICE per feature (note: not RICE of mean inputs) |

## Design baseline

Four pins from Shriya's Pinterest home feed were used as design input. No pin images are embedded, hotlinked or traced. Everything is redrawn in SVG/CSS/canvas, and no characters or logos from the pins appear.

| Page part | Pin | What I took from it |
|---|---|---|
| Hero | [Affirmations poster, by Shei Oliveras](https://www.pinterest.com/pin/406238828912355412/) | Huge condensed all-caps headline in near-black (#1E1E1E) on warm off-white (#EDEBE6), last words in grey, square period accent, a monochrome compass |
| Cards & buttons | [Tech That Actually Makes Life Easier, by oh!](https://www.pinterest.com/pin/1056094181381654521/) | Pale sky gradient (#CFEAFA → white), cobalt (#1F3FD6) monospace `<angle_bracket>` labels mixing bold and italic, a pixel-arrow cursor |
| KPI badges | [Hopecore](https://www.pinterest.com/pin/894949757223897316/) | Royal-blue night sky (#1A2A7A → #2B47B5), cream moon (#F4EED2), mustard monospace numbers (#F2C14E), star specks, a red ground stripe (#C8352B) |
| Simulation room | [Horses under the stars, by Gracielagraciegrace](https://www.pinterest.com/pin/571042427773907103/) | Slate starry sky (#2C3E50 → #5A7184), a snow-white canvas field, motion-streak trails on falling samples |

---

Unofficial concept prototype by Shriya Shekatkar. All data is simulated. Not affiliated with any company.
