# Adding Private Markets to a 60/40 Portfolio

What happens if a 60/40 portfolio puts part of its money into private markets? This project tests that in Python and Excel.

Selina Hach, October 2026

---

## The idea

A 60/40 portfolio holds 60% US stocks and 40% US bonds. It looks balanced, but about 92% of its risk comes from the stocks.

So I tested moving 10%, 20% or 30% of it into four private strategies: private credit, private equity, real estate and infrastructure.

Three questions:
1. What does each strategy do to the portfolio on its own?
2. What is the best way to split the money between them?
3. What changes if the money is locked up for 3 years?

## What I found

**In short:** private markets make the 60/40 better, real estate helps the most, and with a 3-year lockup the portfolio can safely hold about 25%, not 30%.

### 1. Each strategy on its own (20% of the portfolio)

| Strategy | Return | Risk | Sharpe |
|---|---|---|---|
| Real estate | +0.41% | **−1.19%** | **+0.087** |
| Private equity | **+1.04%** | +1.00% | +0.060 |
| Private credit | +0.37% | −0.37% | +0.049 |
| Infrastructure | +0.05% | −1.03% | +0.042 |

*Change vs. the 60/40. Sharpe ratio = return above cash for each unit of risk; higher is better.*

- **Real estate is the best diversifier.** It lowers risk the most and still adds return, because it doesn't move much with stocks.
- **Private equity adds the most return, but it is the only one that adds risk.** It moves a lot like stocks.
- **Infrastructure lowers risk but adds almost no return.**

### 2. Best way to split the private money

| | 60/40 | 10% private | 20% private | 30% private |
|---|---|---|---|---|
| Stocks | 60% | 54% | 48% | 42% |
| Bonds | 40% | 36% | 32% | 28% |
| Real estate | | 8% | 8% | 8% |
| Private equity | | 2% | 7% | 7% |
| Private credit | | | 5% | 9.3% |
| Infrastructure | | | | 5.7% |
| **Expected return** | 6.73% | 7.00% | 7.35% | 7.44% |
| **Risk** | 10.61% | 10.14% | 10.25% | 9.92% |
| **Sharpe** | 0.34 | 0.38 | 0.41 | 0.44 |

- The model fills real estate first, then private equity, then private credit, then infrastructure.
- The Sharpe ratio goes up at every step: from 0.34 to 0.44 at 30% private.
- Stocks still drive most of the risk: about 92% in the 60/40, and still 68% at 30% private.

### 3. With a 3-year lockup

| Private allocation | Fits? | Sharpe (vs. no lockup) |
|---|---|---|
| 10% | Yes | 0.38 (0.38) |
| 20% | Yes | 0.40 (0.41) |
| 30% | **No**, max is 25.6% | n/a |

- Private money can't be sold for 3 years, so its share of the portfolio grows. If real estate or private equity start right at their limits, they go over in 85% to 90% of simulated cases.
- To stay safe, each strategy has to start 22% to 34% below its limit. That brings the total the portfolio can hold down to about 25.6%.

### 4. Same risk, more return

At the same risk as the 60/40 (10.61%):

| | Expected return | Extra vs. 60/40 |
|---|---|---|
| Stocks and bonds only | 6.73% | |
| With private markets | 7.61% | **+0.88% a year** |
| With private markets, 3-year lockup and cash buffer | 7.26% | **+0.53% a year** |

The lockup and cash buffer cost about a third of the benefit, but private markets still come out ahead.

**Bottom line:** what limits how much to put in private markets here is liquidity, not return.

## How it works

**1. Inputs.** Expected return, risk and correlation for each asset come from J.P. Morgan's 2026 long-term forecasts. I used forecasts instead of past data, because private assets are only priced a few times a year, so their history makes them look safer than they are.

**2. One strategy at a time.** I move part of the 60/40 into one private strategy and see how return and risk change. The money comes out of stocks and bonds in the same 60/40 proportion, so at 20% private the portfolio is 48% stocks and 32% bonds.

**3. Best split.** Python tries different ways to split the private money and keeps the one with the highest Sharpe ratio. Each strategy has a maximum weight.

**4. Lockup test.** Private money can't be sold for 3 years, so its share of the portfolio can grow past the limit. Python runs 10,000 possible markets over 3 years to see how much each strategy grows. Starting weights are then lowered so they stay under the limit in at least 95% of cases.

**5. Comparing risk and return.** For every level of risk, I find the best possible return with and without private markets (the efficient frontier).

**6. Checking with real data.** I downloaded prices for public funds that track each strategy (for example, REITs for real estate) and compared them to the J.P. Morgan numbers.

## Real data check

How closely each one moves with stocks (1 = moves exactly together):

| | Public fund | Past 10 years | J.P. Morgan forecast |
|---|---|---|---|
| Bonds | AGG | 0.37 | 0.30 |
| Private credit | BIZD | 0.71 | 0.55 |
| Private equity | PSP | 0.86 | 0.75 |
| Real estate | VNQ | 0.75 | 0.35 |
| Infrastructure | IGF | 0.72 | 0.47 |

The public funds move much more with stocks, partly because they trade on the stock market every day. This is the biggest risk to the results: if private real estate acts more like REITs, it won't lower risk as much as the model says.

## My assumptions

- Starting portfolio: 60% stocks, 40% bonds
- Each private strategy keeps 15% in cash, so investors can withdraw money
- Maximum weight: private credit 12%, real estate 8%, infrastructure 8%, private equity 7%
- No more than 30% in private markets in total
- Lockup: 3 years
- A strategy can go over its limit in at most 5% of simulated cases

These limits are my own choices, not market data.

## What this model doesn't cover

- It uses average manager returns. In private markets, picking a good or bad manager makes a big difference.
- It doesn't model when money is called in or paid out.
- It assumes normal market swings, so it understates the risk of rare crashes.

## Files

| File | What it is |
|---|---|
| `Private_Markets_Allocation_Model.ipynb` | Python notebook with the full analysis |
| `Private_Markets_Allocation_Model.xlsx` | Excel version, with formulas you can check |
| `Private_Markets_Allocation_Deck.pptx` | Slide deck summarizing the results and recommendation |

## How to run

Open the notebook in Google Colab and click **Runtime > Run all**. It takes under a minute and needs no setup.

## Source

J.P. Morgan Asset Management, 2026 Long-Term Capital Market Assumptions (USD), as of September 30, 2025.
