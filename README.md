# INF312_Consumer_Data_Mining_Simulation
# UT Market — Algorithm Explainer Memo (Lecture Notes)

> **INF312: The World Becomes Data** · Consumer Data Mining Simulation (corresponds to `index-v3.html`)
>
> **Core Pedagogical Takeaway**: This simulation contains no machine learning, predictive AI, or real-world dataset training. All scores, weights, heuristics, and classification thresholds are **handcrafted, deterministic rules**. That design choice is itself the central lesson: consumer profiling systems that appear "intelligent" are often simply human engineers and business analysts deciding on categories, weights, and thresholds behind the scenes.

---

## 1. The Six Hidden Product Metrics

Every product in the market catalog carries six hidden dimensions, each scored on a discrete scale from **0 to 3**. These metrics are invisible to the user during gameplay and are used exclusively by the back-end profiling engine.

| Metric | Dimension | A Higher Score Indicates |
|:---|:---|:---|
| `campus` | Campus / Academic Lifestyle | Strong signal of student identity or coursework requirements |
| `entertainment` | Entertainment Orientation | Strong alignment with leisure, recreation, and experiential consumption |
| `brand` | Brand / Premium Affinity | Preference for recognizable branding, prestige, or status markers |
| `priceConsc` | Frugality / Price Consciousness | Deliberate, budget-conscious decision-making |
| `promo` | Promotion Responsiveness | High sensitivity to markdowns, bundles, and discount mechanisms |
| `discretionary` | Want vs. Need | Skews toward discretionary indulgence ("want"); 0 indicates a strict staple ("need") |

An additional boolean flag, `premium` (`true`/`false`), identifies high-margin, luxury, or status-oriented products.

### Core Principles for Classroom Discussion
- **Expensive $\neq$ High Brand Affinity**: An expensive utilitarian tool does not automatically signal status seeking.
- **Inexpensive $\neq$ High Price Consciousness**: Buying cheap goods does not always signify deliberate frugality.
- Metrics are assigned based on the **behavioral signal a purchase communicates**, not raw price points alone.

---

## 2. Product Catalog: All 28 Items & Behavioral Rationales

*Column Abbreviations: Camp = Campus · Ent = Entertainment · Brand = Brand Affinity · Price = Price Consciousness · Promo = Promotion Responsiveness · Disc = Discretionary · Prem = Premium Flag*

### 📚 Study & Campus
| Product | Price | Camp | Ent | Brand | Price | Promo | Disc | Prem | Rationale |
|---|---|:---:|:---:|:---:|:---:|:---:|:---:|:---:|---|
| U of T × Hello Kitty T-Shirt | 100 | 3 | 0 | 2 | 2 | 2 | 2 | ✗ | Strong campus identity with deliberate ambiguity: fandom appeal meets daily utility; moderate price projects light thrift. |
| U of T Hoodie | 300 | 3 | 0 | 2 | 0 | 1 | 1 | ✓ | High institutional identification; elevated price point eliminates budget signal; functional semi-essential. |
| ChatGPT Plus | 120 | 2 | 1 | 2 | 2 | 3 | 1 | ✗ | Deliberately ambiguous across study, work, and leisure; recurring SaaS format implies high promotion sensitivity. |
| Notebook Bundle | 90 | 2 | 0 | 1 | 2 | 0 | 0 | ✗ | Utilitarian academic staple: cheap, functional, devoid of entertainment or promotional appeal. |
| Noise-Canceling Headphones | 420 | 2 | 1 | 3 | 0 | 1 | 1 | ✓ | Study-functional, but carries strong brand cachet; deliberate non-budget purchase. |
| Laptop Sleeve | 70 | 2 | 0 | 1 | 2 | 1 | 0 | ✗ | Inexpensive, functional campus baseline essential. |

### 🎮 Entertainment
| Product | Price | Camp | Ent | Brand | Price | Promo | Disc | Prem | Rationale |
|---|---|:---:|:---:|:---:|:---:|:---:|:---:|:---:|---|
| Concert Ticket | 500 | 0 | 3 | 1 | 0 | 2 | 3 | ✓ | High-ticket experiential purchase: pure leisure, peak discretionary consumption. |
| Cineplex Movie Ticket | 120 | 0 | 3 | 1 | 2 | 2 | 2 | ✗ | Accessible mainstream leisure; budget-conscious leisure benchmark. |
| CineClub Membership | 100 | 0 | 3 | 1 | 2 | 3 | 2 | ✗ | Recurring subscription: signals peak promotional interest and routine entertainment spend. |
| F1 Grand Prix Ticket | 700 | 0 | 3 | 2 | 0 | 1 | 3 | ✓ | Highest-priced single SKU in the catalog: used to demonstrate opportunity cost and pure luxury consumption. |
| Soccer Match Ticket | 350 | 0 | 3 | 1 | 1 | 1 | 3 | ✗ | Mid-tier experiential live entertainment. |
| Gaming Currency | 200 | 0 | 3 | 0 | 1 | 3 | 3 | ✗ | Unbranded digital consumable; high promotional responsiveness, completely discretionary. |

### 🧘 Lifestyle
| Product | Price | Camp | Ent | Brand | Price | Promo | Disc | Prem | Rationale |
|---|---|:---:|:---:|:---:|:---:|:---:|:---:|:---:|---|
| iPhone 18 Pro | 850 | 2 | 0 | 3 | 0 | 1 | 1 | ✓ | **Core Demo Item**: Most expensive item, yet `Disc = 1` (a near-essential communication utility for students). `Price = 0` reflects an absence of thrift signal, not merely the high price tag. |
| AirPods Pro 3 | 300 | 1 | 2 | 3 | 0 | 2 | 2 | ✓ | Premium brand status blended with personal entertainment and study utility. |
| lululemon Align Pants | 150 | 1 | 0 | 3 | 1 | 2 | 2 | ✓ | High brand prestige at an accessible mid-tier price point; demonstrates that strong brand affinity does not entirely cancel thrift signals. |
| Instant Camera | 300 | 1 | 2 | 2 | 0 | 1 | 3 | ✓ | Trend-driven novelty hardware; purely discretionary lifestyle purchase. |
| Campus Tumbler | 140 | 2 | 0 | 2 | 1 | 1 | 1 | ✗ | Everyday student functional gear with moderate lifestyle branding. |
| Designer Backpack | 450 | 2 | 0 | 3 | 0 | 1 | 2 | ✓ | Premium lifestyle fashion applied to functional student gear. |

### 🎁 Fun & Treats
| Product | Price | Camp | Ent | Brand | Price | Promo | Disc | Prem | Rationale |
|---|---|:---:|:---:|:---:|:---:|:---:|:---:|:---:|---|
| Jellycat Plush Toy | 120 | 0 | 1 | 2 | 1 | 2 | 3 | ✓ | Collectible, brand-conscious impulse purchase; purely discretionary. |
| Mystery Blind Box | 150 | 0 | 2 | 2 | 1 | 3 | 3 | ✗ | Gamified consumption mechanic: peak promo sensitivity, pure personal indulgence. |
| Gourmet Pastry Set | 100 | 0 | 1 | 2 | 0 | 2 | 3 | ✓ | Short-term sensory indulgence; micro-luxury expenditure. |
| Collectible Figurine | 220 | 0 | 2 | 3 | 0 | 2 | 3 | ✓ | High-status fandom collectible; entirely discretionary. |
| Snack Box | 80 | 0 | 1 | 1 | 2 | 2 | 3 | ✗ | Low-cost casual indulgence: cheap and non-essential. |

### 🛒 Everyday Essentials *(Intentionally unbranded to serve as experimental controls)*
| Product | Price | Camp | Ent | Brand | Price | Promo | Disc | Prem | Rationale |
|---|---|:---:|:---:|:---:|:---:|:---:|:---:|:---:|---|
| Drip Coffee Set | 80 | 1 | 0 | 1 | 2 | 1 | 1 | ✗ | Routine daily staple: accessible, pragmatic. |
| Pen Set | 40 | 1 | 0 | 1 | 3 | 0 | 0 | ✗ | Archetypal utilitarian baseline: low cost, zero leisure value, pure academic necessity. |
| Household Essentials | 60 | 0 | 0 | 0 | 3 | 1 | 0 | ✗ | **The catalog's only `Brand = 0` item**: functions as the pure utilitarian control baseline. |
| Phone Charger | 50 | 1 | 0 | 1 | 3 | 1 | 0 | ✗ | Inexpensive, functional household hardware. |
| Canvas Tote Bag | 45 | 1 | 0 | 1 | 3 | 1 | 1 | ✗ | Utilitarian, cheap, everyday utility item. |

**Experimental Control Setup**:
- `Everyday Essentials` items uniformly hold `Price = 3` and `Disc = 0`.
- `Fun & Treats` items uniformly hold `Disc = 3`.
- This ensures that consumer intent (functional necessity versus indulgence) is cleanly differentiated through observable behavioral choice.

---

## 3. Profile Dimension Scoring Formulas (0–100)

**Underlying Philosophy**: Scores prioritize **empirically observed behavior** (budget allocation, unit volume, coupon engagement), using underlying product metadata only as a secondary weighting layer. Each dimension normalizes independently to prevent flat or inflated scores. When behavioral data is scarce, scores gravitate toward moderate baselines rather than generating artificial extremes.

### Metadata Normalization
Let $\text{metaAvg}(k)$ represent the volume-weighted average of metadata attribute $k$ across all items purchased by the user, normalized to a $[0, 1]$ interval:

$$\text{metaAvg}(k) = \frac{\sum_{i} (\text{quantity}_i \times \text{metric}_{i, k})}{3 \times \sum_{i} \text{quantity}_i}$$

---

### 1. Campus Lifestyle
```text
100 * [ 0.55 * (study_spend_share + 0.6 * everyday_spend_share) + 0.45 * metaAvg("campus") ]
```
*Rationale: Campus identity is primarily signaled by budget commitment to academic and everyday student essentials, reinforced by campus-specific product attributes.*

### 2. Entertainment Orientation
```text
100 * [ 0.60 * entertainment_spend_share + 0.20 * min(1, entertainment_sku_count / 2) + 0.20 * metaAvg("entertainment") ]
```
*Rationale: Budget share is given the dominant weight ($0.60$), as allocating limited capital to leisure represents the strongest behavioral signal.*

### 3. Brand Orientation
```text
100 * [ 0.55 * premium_spend_share + 0.45 * metaAvg("brand") ]
```
*Rationale: Focuses specifically on the proportion of capital allocated to designated `premium` goods rather than aggregate gross spending.*

### 4. Price-Conscious Purchasing *(Requires balanced weighting)*
```text
100 * [ 0.45 * budget_tier_item_share + 0.30 * basket_discount_vs_catalog_avg + 0.25 * coupon_redemption_rate ]
```
*Rationale: Combines **three distinct signals** so that purchasing a single high-ticket item does not instantly zero out a user's frugality score (e.g., purchasing an iPhone alongside cheap coffee still maintains a score of ~35).*

### 5. Promotion Responsiveness
```text
100 * [ 0.40 * offer_redemption_rate + 0.25 * discounted_item_share + 0.15 * min(1, promo_repeat_purchases / 2) + 0.20 * metaAvg("promo") ]
```
*Rationale: Directly quantifies active discount-seeking behaviors across coupon redemptions, marked-down selections, and repeat promotional engagement.*

---

## 4. Consumer Archetypes: Pattern-Based Classification

Archetypes are evaluated sequentially using an `if / else if` hierarchy. **The first matching condition is assigned**. Thresholds require a distinct dominant behavioral pattern:

| Archetype | Evaluation Rule |
|:---|:---|
| **The Experience Seeker** | `Entertainment >= 55` AND `Discretionary Spend Share >= 0.45` |
| **The Deal Optimizer** | `Promotion >= 55` AND (`Offers Redeemed >= 2` OR `Discounted Items >= 2`) |
| **The Treat Hunter** | `Fun Category Spend Share >= 0.35` AND `Discretionary >= 0.50` |
| **The Lifestyle Curator** | `Brand >= 55` AND `Lifestyle Category Spend Share >= 0.30` |
| **The Practical Planner** | `Campus >= 50` AND `Price-Conscious >= 50` AND `Discretionary Spend Share <= 0.35` |
| **The Balanced Shopper** | Default fallback when no single dominant pattern emerges |

---

## 5. Inference Confidence: Designed Pedagogical Provocation

Confidence is qualitatively categorized according to the volume and diversity of observed behavioral evidence (total units, distinct categories, and SKU diversity):

$$\text{None} \longrightarrow \text{Limited} \longrightarrow \text{Moderate} \longrightarrow \text{More}$$

**Pedagogical Intent**: The dashboard presents an authoritative, highly specific consumer profile even when the user has only made one or two simple purchases. This design prompts students to reflect on **the illusion of certainty manufactured by datafication**—how readily digital systems convert sparse, noisy inputs into confident algorithmic judgments.

---

## 6. Verified Test Scenarios

These behavioral test paths have been verified via headless JavaScript execution:

| Scenario | Cart Contents | Output Archetype | Key Metric Highlights |
|:---|:---|:---|:---|
| **A: Tech Splurge** | iPhone + Drip Coffee Set | Lifestyle Curator | Brand: 81; Price-Conscious: **35** *(not collapsed to 0 by luxury purchase)* |
| **B: Experience Seeker** | F1 Ticket + Cineplex + Drip Coffee | Experience Seeker | Entertainment: 88; Discretionary Spend Share: 0.92 |
| **C: Brand & Lifestyle** | AirPods + lululemon + Jellycat | Deal Optimizer | Brand: 95; Discretionary Spend Share: 1.00 |
| **D: Pragmatic Student** | Notebook + Sleeve + Pen + Coffee + Household | Practical Planner | Campus: 62; Price-Conscious: 80; Entertainment: 0 |
| **E: Promo Driven** | 3 Redeemed Offers | Deal Optimizer | Promotion: 88; Redemptions: 3 |

---

## 7. Critical Theory Connections (Lecture Alignment)

- **Datafication**: The conversion of ordinary, contextual human choices into quantifiable, commodified data assets.
- **Inferential Privacy**: The tension between explicit disclosure and behavioral extraction—exemplified by the simulation's reveal banner: *"You didn't tell us most of this. We inferred it."*
- **The Target Pregnancy Prediction Parable**: This project acts as an accessible analog to corporate algorithmic profiling, showing how proxy variables are leveraged to simulate predictive insight.
- **The Rhetorical Authority of Dashboards**: The presentation of clean visual metrics from sparse behavioral traces demonstrates how computational platforms manufacture perceived objectivity and truth.

---

## 8. Customization & Maintenance Guide

- **Modify Products, Prices, or Attributes**: In `index-v3.html`, update the `PRODUCTS` array (adjust `meta: {...}` values and `price`).
- **Update Personalized Offers**: Edit the `OFFER_CANDIDATES` array.
- **Adjust Scoring Weights & Classification Logic**: Tune `calculateProfileSignals()` and `determineArchetype()`.
- **Deploy Changes**: Save `index-v3.html` and refresh the browser. The project requires no build pipeline, compiler, or external package manager.

*(Note: These lecture notes correspond directly to `index-v3.html`. Legacy files `index.html` and `index-v2.html` are preserved for version history.)*
