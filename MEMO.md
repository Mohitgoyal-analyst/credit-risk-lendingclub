# Loan Selection on LendingClub: Grades Are Not a Reliable Price Signal

**Analysis of 786,820 matured loans issued 2012–2015**

---

## Recommendation

Stop buying loans by grade. Score borrowers individually and buy the safest quintile within the higher-yield grades, where the platform's own assessment is weakest and the return spread is widest.

On out-of-sample 2015 loans, this approach identified a subset of grade D loans returning **3.49% annually** — outperforming the average grade B loan at 2.45%, despite grade D's higher headline default rate.

---

## Three findings

**1. The risk premium barely compensates.**

On fully matured 2012–2013 loans, realised annual returns run from 2.52% (grade A) to 4.29% (grade E) — a modest reward for a five-fold increase in default rate. Every grade returns far below its advertised rate: grade A advertises 7.7% and delivers 2.52%. Defaults alone cannot explain that gap at a 5.7% default rate. Prepayment does — borrowers repay early without penalty, and capital stops earning.

**2. Grade letters drifted materially over time.**

Restricting to 36-month loans, which are fully matured across the whole period, a grade E borrower defaulted at 23.0% for 2012-13 and 30.4% for 2014-15. Same letter, same term, both cohorts fully resolved. As LendingClub scaled from 43,000 to 283,000 loans a year, the risk behind each letter rose while the label stayed fixed. A grade is therefore not comparable across vintages.

**3. Borrower attributes outperform the platform's own grade.**

Trained on 2012–2014 and tested on 2015, a model using only origination-time borrower data achieves **AUC 0.736** against **0.715** for LendingClub's own interest rate and sub-grade. Adding their grade to the model improves it only marginally (0.742), indicating their assessment captured little the raw attributes did not.

---

## The commercial case

Within grade D — where every borrower was charged the same rate — the model separates outcomes sharply:

| Quintile by predicted risk | Default rate | Realised annual return |
|---|---|---|
| Safest | 16.7% | **3.49%** |
| Middle | 25.8% | 1.94% |
| Riskiest | 35.8% | 0.11% |

A 3.4-point spread in realised return at a single uniform price. The pattern holds across grades B through E and widens as grade worsens — the platform's assessment was least reliable exactly where the stakes were highest.

At the portfolio level, approving the safest 30–50% of applicants returns roughly **2.71%** against **2.19%** for approving all. The curve is flat across that range, so the cutoff can be set on volume or operational grounds without material cost.

---

## What we are not claiming

**This does not support lending to more people.** Every loan analysed had already passed LendingClub's approval filter. We cannot observe outcomes for rejected applicants, so the finding is strictly about choosing better among those already approved — not about extending credit further.

**Defaults are partial losses, not total ones.** Charged-off loans recover 33% of principal on average; only 0.1% of defaulters paid nothing. Loss is approximately 38 cents per dollar lent, which is why high default rates do not translate proportionally into lost capital.

---

## Limitations

- **No recession in the data.** 2012–2015 was a benign credit environment. Credit models typically degrade most under stress, which is when they matter most.
- **Return figures are simplified.** Annualised without discounting cash flows within a loan; a default in month 30 costs less than one in month 3. Servicing fees are excluded, overstating all returns by a similar margin.
- **Maturity bias.** Return analysis is restricted to 36-month loans, which are fully resolved. Longer-term loans issued after 2013 over-represent early defaults and were excluded from return calculations for that reason.
- **Single platform, single product.** Findings describe LendingClub's unsecured personal loans and should not be assumed to generalise.

---

## Method note

Of 151 available columns, approximately 40 record events during or after repayment — payments received, recoveries, settlements. These were excluded from the model, as they do not exist at the point of the lending decision; including them produces near-perfect accuracy with no predictive value. A further 14 columns were excluded because LendingClub began collecting them only in late 2015, making their presence a proxy for issue date rather than borrower quality.

Validation used a time-based split (train 2012–2014, test 2015) rather than a random one. Default rates rose from 17.2% to 20.2% across that boundary; a random split would have allowed the model to learn the later period's conditions and overstated performance.
