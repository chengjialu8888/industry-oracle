# Output Quality Calibration Examples

> **Loading rule**: Load when generating any Step output document for quality self-check.

---

## Purpose

These examples calibrate the **tone, depth, and specificity** expected in Industry Oracle outputs. Before finalizing any deliverable, compare your draft against the relevant Good/Bad pair below.

---

## Example 1: Competitor Insight Extraction (Step 1)

### Bad ❌
> "Company A is a leading player in the AI customer service market. They have a strong product and growing revenue. Their main advantage is their technology, and their weakness is that they are still burning cash."

**Why it's bad**: Generic, could describe any startup. No specific mechanisms, no numbers, no non-obvious insight.

### Good ✅
> "Company A's actual moat is not their NLP model (which benchmarks show trails GPT-4 by 12% on CSAT prediction) — it's their 340+ pre-built integration connectors with enterprise CRMs and ticketing systems. Switching cost is 6-9 months of re-implementation. Their Q3 2024 filing reveals net dollar retention of 127%, but new logo acquisition slowed to 14 per quarter (vs. 23 in Q1). This suggests strong product-market fit with existing customers but increasing difficulty in initial sales — likely because their $180K ACV prices out the mid-market segment that represents 70% of remaining TAM."

**Why it's good**: Specific metrics, non-obvious insight (the moat isn't where you'd expect), identifies a specific tension (retention vs. acquisition), quantified TAM implication.

---

## Example 2: Assumption Deconstruction (Step 2)

### Bad ❌
> "**Assumption**: The market is large.  
> **Risk**: The market might be smaller than expected.  
> **Mitigation**: Do more market research."

**Why it's bad**: Circular. States the obvious. No falsifiable claim. No specific evidence. Mitigation is a platitude.

### Good ✅
> "**Assumption**: Enterprise procurement teams will adopt AI-generated RFP responses without human review within 18 months.  
> **Confidence**: 25% (Low)  
> **Evidence against**: (1) Gartner's 2024 survey shows 89% of procurement leaders require human sign-off on documents exceeding $50K value — this threshold hasn't budged in 3 years despite AI tool availability. (2) Regulatory precedent: EU AI Act Article 14 mandates human oversight for AI systems affecting contractual relationships. (3) Liability gap: if an AI-generated RFP response contains a commitment the vendor can't fulfill, current legal frameworks hold the signing human liable — creating strong incentive for review.  
> **What would change this**: Adoption will accelerate only when (a) insurance products cover AI-generated contractual errors, OR (b) a major procurement platform (SAP Ariba, Coupa) embeds auto-approval as a default feature, shifting the Overton window.  
> **If false**: The startup's TAM shrinks by ~60%, as 'human-in-the-loop' usage drives 3× lower usage frequency and 40% lower WTP."

**Why it's good**: Specific, falsifiable assumption with timeline. Multi-source evidence. Clear trigger conditions. Quantified business impact if wrong.

---

## Example 3: Investor Pressure Test Questions (Step 3)

### Bad ❌
> "An investor might ask: 'What's your competitive advantage?' and 'How big is your market?'"

**Why it's bad**: These are generic interview questions, not pressure-test questions. They don't probe specific vulnerabilities.

### Good ✅
> "**[Peter Thiel lens]**: 'You claim your proprietary dataset of 2M customer support tickets is a moat. But Zendesk has 100M+ tickets. What stops them from training a model on day one that's 50× better than yours? Your 2M tickets aren't a secret — they're a rounding error in Zendesk's database.'
>
> **[Munger inversion lens]**: 'Your model assumes 15% monthly churn in SMB. The industry base rate for SMB SaaS churn is 3-5% monthly. If your churn hits 8% — still better than your assumption — your LTV:CAC drops from 4.2 to 1.8, below viability. Why should I believe you'll achieve churn rates that even Slack couldn't maintain in their SMB segment?'"

**Why it's good**: Each question is specific to this particular startup's claims, invokes a named investor lens, contains specific numbers that challenge the thesis, and points to a concrete business consequence.

---

## Example 4: Debate Round Output (Step 4)

### Bad ❌
> "The bull case is that the market is big and the team is strong. The bear case is that competition is fierce. After consideration, the truth is somewhere in the middle."

**Why it's bad**: "Truth is somewhere in the middle" is intellectual laziness. No specific claims advanced or refuted. No synthesis.

### Good ✅
> "**Bull's strongest surviving argument**: The regulatory moat is real — obtaining HIPAA BAA certification took Company X 14 months and $2.3M, and only 3 competitors have it. In healthcare AI, this is a genuine barrier, not a checkbox. New entrants face the same timeline regardless of their technology advantage.
>
> **Bear's strongest surviving argument**: The 'AI doctor' positioning is premature. Current clinical validation covers only 4 of the 23 specialties they claim on their website. Expanding clinical validation at their current pace (1 specialty per quarter) means full coverage takes until 2029 — by which time at least 2 Big Tech health divisions will have completed their own validation studies.
>
> **Synthesis — not a midpoint, but a conditional**: The company is investable IF it (1) narrows its positioning to the 4 validated specialties, which alone represent a $1.2B SAM, and (2) secures a partnership with a top-5 hospital system for co-validation within 12 months, which would accelerate the timeline from 5 years to ~2 years via parallel validation tracks. Without condition (2), the bear thesis dominates."

**Why it's good**: Identifies the strongest argument from each side (not all arguments). Synthesis is conditional, not a cop-out "middle ground." Specific numbers and timeline. Clear actionable next step.

---

## Writing Style Calibration

| Dimension | ❌ Avoid | ✅ Target |
|-----------|---------|----------|
| **Specificity** | "The market is large" | "The addressable market is $3.2B (IDC 2024), growing 23% CAGR" |
| **Insight depth** | "They have a good team" | "3 of 5 founders previously built the ranking system at Company Y that increased conversion 34%" |
| **Hedging** | "This might potentially possibly work" | "Confidence: 60%. Key dependency: regulatory approval timeline" |
| **Actionability** | "They should think about pricing" | "Test price point at $49/mo (current) vs $99/mo with annual commit — the 2× gap is where SMB willingness-to-pay typically plateaus in this vertical" |
| **Language** | Mixing languages randomly | **Primary language follows user's input.** Technical terms keep original English form (TAM, LTV, PMF). Chinese context uses Chinese with English terms inline |

---

## Language Convention

- **Follow the user's primary language** for all output
- **Technical terms**: Keep canonical English forms — TAM, SAM, SOM, LTV, CAC, PMF, JTBD, NPS, ARR, MRR, CAGR, EBITDA
- **In Chinese context**: 使用中文行文，技术术语保留英文原文（如 "净收入留存率 (Net Dollar Retention) 达到 127%"）
- **In English context**: Write fully in English; no Chinese unless quoting Chinese-language sources
- **Numbers**: Always use Arabic numerals with units. "$3.2B" not "thirty-two billion dollars"
