# Marketing Analytics Platform - Complete Guide
## Understanding MMM, MTA, and Incrementality in Simple Terms

---

## 📚 Table of Contents

1. [What is This Platform?](#what-is-this-platform)
2. [The Three Pillars of Marketing Measurement](#the-three-pillars)
3. [Marketing Mix Modeling (MMM)](#marketing-mix-modeling-mmm)
4. [Multi-Touch Attribution (MTA)](#multi-touch-attribution-mta)
5. [Incrementality Testing](#incrementality-testing)
6. [Triangulation - Combining All Three](#triangulation)
7. [Data Transformations](#data-transformations)
8. [Budget Optimization](#budget-optimization)
9. [Glossary of Terms](#glossary)

---

## What is This Platform? {#what-is-this-platform}

Imagine you're a business owner spending money on different types of advertising:
- TV commercials
- Google ads
- Facebook ads
- Billboards
- Email campaigns

**The big question is:** *Which of these is actually bringing in customers and sales?*

This platform answers that question using three different scientific approaches, then combines them to give you the most accurate picture possible.

### Why Three Approaches?

Think of it like getting a medical diagnosis:
- **One doctor** might use blood tests (MMM)
- **Another doctor** might use X-rays (MTA)
- **A third doctor** might run experiments (Incrementality)

When all three agree, you can be very confident in the diagnosis. This is called **Triangulation**.

---

## The Three Pillars of Marketing Measurement {#the-three-pillars}

| Approach | What It Does | Best For | Limitation |
|----------|--------------|----------|------------|
| **MMM** | Looks at historical data to find patterns | Long-term strategic planning | Can't see individual customers |
| **MTA** | Tracks individual customer journeys | Digital channel optimization | Doesn't work for TV/Radio |
| **Incrementality** | Runs experiments to prove causation | Proving something truly works | Requires controlled tests |

---

## Marketing Mix Modeling (MMM) {#marketing-mix-modeling-mmm}

### What is MMM in Simple Terms?

MMM is like a detective looking at your company's history to figure out what drove sales.

**Simple Example:**
Imagine you have 2 years of data showing:
- How much you spent on each advertising channel each week
- How many sales you made each week
- Other factors like seasonality, promotions, weather

MMM uses statistics to figure out: *"When TV spending went up, did sales go up too? By how much?"*

### The Three MMM Models Available

#### 1. Ridge Regression (Recommended for Beginners)

**What it is:** A mathematical formula that finds the relationship between your spending and sales.

**Analogy:** It's like drawing the best-fit line through a scatter plot, but smarter - it prevents any single channel from looking too good or too bad.

**When to use:** 
- ✅ Quick analysis needed
- ✅ Limited data (less than 2 years)
- ✅ First-time MMM users

**How it works:**
```
Sales = Base Sales + (TV effect × TV Spend) + (Google effect × Google Spend) + ...
```

The "effect" numbers are what MMM calculates. A higher effect means that channel is more impactful.

#### 2. Bayesian MCMC (For Advanced Users)

**What it is:** A more sophisticated approach that gives you a range of possible answers instead of just one number.

**Analogy:** Instead of saying "TV drives $50,000 in sales," it says "TV drives somewhere between $40,000 and $60,000 in sales, with $50,000 being most likely."

**When to use:**
- ✅ Need uncertainty estimates
- ✅ Have prior knowledge about channel performance
- ✅ Want to combine expert intuition with data

**Note:** Requires PyMC library to be installed.

#### 3. Multiplicative (Log-Log) Model

**What it is:** Assumes that marketing has diminishing returns (the more you spend, the less each additional dollar helps).

**Analogy:** Like eating pizza - the first slice is amazing, the fifth slice is okay, the tenth slice might make you sick. More isn't always proportionally better.

**When to use:**
- ✅ Believe channels have saturation effects
- ✅ Want to model diminishing returns naturally
- ✅ Channels have very different spending levels

### Key MMM Outputs

| Output | What It Means |
|--------|---------------|
| **Contribution** | How much sales each channel drove (in dollars) |
| **ROI (Return on Investment)** | For every $1 spent, how many dollars of sales returned |
| **Coefficient** | The "strength" of each channel's impact |
| **R² Score** | How well the model explains your sales (0-100%, higher is better) |

### Example MMM Results Interpretation

```
Channel      | Spend    | Contribution | ROI
-------------|----------|--------------|-----
TV           | $100,000 | $250,000     | 2.5
Google Ads   | $50,000  | $175,000     | 3.5
Facebook     | $30,000  | $60,000      | 2.0
```

**What this tells you:**
- Google Ads has the best ROI (3.5x return)
- TV drives the most total value but less efficiently
- Facebook is underperforming relative to other channels

---

## Multi-Touch Attribution (MTA) {#multi-touch-attribution-mta}

### What is MTA in Simple Terms?

MTA tracks the journey each customer takes before buying, then assigns credit to each touchpoint.

**Simple Example:**
A customer's journey:
1. Sees a Facebook ad (Day 1)
2. Clicks a Google search ad (Day 5)
3. Opens an email (Day 7)
4. Makes a purchase (Day 8)

**The question:** Which touchpoint deserves credit for the sale?

### The Seven Attribution Models Available

#### 1. First Touch Attribution

**How it works:** 100% of the credit goes to the first interaction.

**Example:** Facebook gets 100%, Google and Email get 0%.

**Best for:** Understanding which channels attract new customers.

**Limitation:** Ignores everything that happened after first contact.

```
Facebook ████████████████████ 100%
Google   
Email    
```

#### 2. Last Touch Attribution

**How it works:** 100% of the credit goes to the final interaction before purchase.

**Example:** Email gets 100%, Facebook and Google get 0%.

**Best for:** Understanding what closes the deal.

**Limitation:** Ignores all the work done to bring the customer to that point.

```
Facebook 
Google   
Email    ████████████████████ 100%
```

#### 3. Linear Attribution

**How it works:** Credit is split equally among all touchpoints.

**Example:** Facebook, Google, and Email each get 33.3%.

**Best for:** When you believe every interaction matters equally.

**Limitation:** Probably not realistic - some touches likely matter more.

```
Facebook ███████ 33.3%
Google   ███████ 33.3%
Email    ███████ 33.3%
```

#### 4. Time Decay Attribution

**How it works:** Touchpoints closer to the purchase get more credit.

**Example:** 
- Facebook (Day 1): 10%
- Google (Day 5): 30%
- Email (Day 7): 60%

**Best for:** When you believe recent interactions are more influential.

**The decay factor:** Controls how quickly credit fades. Higher decay = more emphasis on recent touches.

```
Facebook ██ 10%
Google   ██████ 30%
Email    ████████████ 60%
```

#### 5. Position-Based (U-Shaped) Attribution

**How it works:** First and last touchpoints get 40% each, middle touches share 20%.

**Example:**
- Facebook (first): 40%
- Google (middle): 20%
- Email (last): 40%

**Best for:** Valuing both discovery (first) and conversion (last).

```
Facebook ████████ 40%
Google   ████ 20%
Email    ████████ 40%
```

#### 6. Markov Chain Attribution

**What it is:** A data-driven model that calculates how much removing each channel would hurt conversions.

**Analogy:** Instead of using rules, it looks at actual customer paths to see which channels are truly essential.

**How it works:**
1. Builds a map of how customers move between channels
2. Calculates "removal effect" - what happens if we delete a channel from all paths?
3. Channels with higher removal effects are more important

**Best for:** 
- ✅ Data-driven decisions (no arbitrary rules)
- ✅ When you have lots of customer journey data
- ✅ Discovering hidden channel importance

**Example output:**
```
If we removed Google: 45% of conversions would be lost
If we removed Facebook: 30% of conversions would be lost
If we removed Email: 25% of conversions would be lost
```

#### 7. Shapley Value Attribution

**What it is:** Based on game theory - calculates the fair contribution of each channel.

**Analogy:** Like splitting a restaurant bill fairly based on what each person ordered, not just dividing equally.

**How it works:**
1. Considers all possible combinations of channels
2. Calculates the marginal contribution of adding each channel
3. Averages across all possible orderings

**Best for:**
- ✅ Most mathematically fair attribution
- ✅ Academic rigor required
- ✅ When channels have complex interactions

**Note:** Computationally intensive - works best with fewer channels.

### MTA Model Comparison

| Model | Data-Driven? | Complexity | Best For |
|-------|--------------|------------|----------|
| First Touch | No | Simple | Top-of-funnel analysis |
| Last Touch | No | Simple | Conversion optimization |
| Linear | No | Simple | Equal channel philosophy |
| Time Decay | No | Medium | Recency-focused analysis |
| Position-Based | No | Medium | Balanced view |
| Markov Chain | Yes | Complex | Accurate channel importance |
| Shapley Value | Yes | Complex | Fair value distribution |

---

## Incrementality Testing {#incrementality-testing}

### What is Incrementality in Simple Terms?

Incrementality answers the question: *"Would these sales have happened anyway, even without the ad?"*

**The Problem with Other Methods:**
MMM and MTA can show correlation, but not causation. Just because sales went up when you ran ads doesn't mean the ads caused the sales.

**Incrementality runs actual experiments to prove causation.**

### The Four Incrementality Methods Available

#### 1. Geo Experiments (Geographic Testing)

**What it is:** Run your campaign in some cities/regions but not others, then compare results.

**Simple Example:**
- **Test Group:** Run Facebook ads in New York, Chicago, LA
- **Control Group:** No Facebook ads in Boston, Seattle, Denver
- **Compare:** Did sales grow more in test cities?

**How it works (Difference-in-Differences):**
1. Measure sales in both groups BEFORE the test
2. Run the campaign only in test cities
3. Measure sales in both groups AFTER the test
4. Calculate: (Test After - Test Before) - (Control After - Control Before)

**Why this works:** The control group shows what would have happened naturally (seasonality, trends), so we can isolate the true campaign effect.

**Best for:**
- ✅ Testing TV, Radio, Billboards (non-digital)
- ✅ Large-scale campaign validation
- ✅ When you can control by geography

#### 2. Synthetic Control

**What it is:** When you can't have a clean control group, we mathematically create one.

**Analogy:** If you only ran ads in California, we create a "Fake California" using a weighted combination of other states that historically behaved like California.

**How it works:**
1. Find regions that historically looked similar to your test region
2. Combine them with weights to match your test region's pre-campaign pattern
3. After campaign: Compare actual test region to synthetic version
4. The difference = campaign impact

**Best for:**
- ✅ When true control groups aren't possible
- ✅ Single-region tests
- ✅ Policy or major strategy changes

**Example:**
```
Actual California Sales (after campaign): $10M
Synthetic California (what would have happened): $8M
Incremental Lift: $2M (25% increase)
```

#### 3. Holdout Tests

**What it is:** Randomly split your audience - show ads to some, not to others.

**Simple Example:**
- **Test Group (90%):** See your Facebook ads
- **Holdout Group (10%):** Never see your Facebook ads
- **Compare:** Conversion rates between groups

**How it works:**
1. Randomly assign users to test or holdout
2. Run campaign only to test group
3. Measure conversion rates for both groups
4. Difference = true incremental impact

**Best for:**
- ✅ Digital campaigns
- ✅ Precise measurement
- ✅ When you can control ad delivery

**Key Metrics:**
- **Lift:** How much better did test group perform?
- **Incremental Conversions:** Extra conversions caused by ads
- **iROAS:** Incremental Return on Ad Spend

#### 4. Time-Based Switching

**What it is:** Turn a channel on and off over time, observe the impact.

**Simple Example:**
- Week 1-2: TV ads ON
- Week 3-4: TV ads OFF
- Week 5-6: TV ads ON
- Compare sales between ON and OFF periods

**How it works:**
1. Define ON periods and OFF periods
2. Collect performance metrics for each
3. Calculate average performance difference
4. Account for seasonality and trends

**Best for:**
- ✅ When geographic or audience splits aren't possible
- ✅ Single-channel tests
- ✅ Measuring lingering effects (carryover)

**Caution:** Make sure ON/OFF periods are comparable (don't test during holidays!).

### Incrementality Metrics Explained

| Metric | What It Means | Formula |
|--------|---------------|---------|
| **Lift** | % improvement from the campaign | (Test - Control) / Control × 100% |
| **Incremental Conversions** | Extra sales/conversions from campaign | Test Conversions - Expected Conversions |
| **iROAS** | Return on incremental spend | Incremental Revenue / Campaign Spend |
| **Statistical Significance** | Confidence the result is real | p-value < 0.05 = confident |

---

## Triangulation - Combining All Three {#triangulation}

### Why Triangulation Matters

Each measurement method has blind spots:

| Method | What It Misses |
|--------|----------------|
| MMM | Can't track individual customers; may miss causation |
| MTA | Only works for digital; ignores offline influence |
| Incrementality | Only measures tested scenarios; time-consuming |

**Triangulation** combines insights from all three to get a complete, validated picture.

### How Triangulation Works

1. **Run all three analyses** on the same channels
2. **Compare the results** - do they agree?
3. **Weight the answers** based on confidence
4. **Identify discrepancies** - investigate where methods disagree

### Example Triangulation Output

```
Channel: Google Search Ads

MMM says:         ROI = 3.2x, Attribution = 25%
MTA says:         Attribution = 28% (Markov), 22% (Shapley)
Incrementality:   Lift = 15%, iROAS = 2.8x

TRIANGULATED VIEW:
✅ All methods agree Google is high-performing
   Confidence: HIGH
   Recommended Attribution: 25% (±3%)
   Recommended ROI: 3.0x (±0.3)
```

### When Methods Disagree

**Scenario:** MMM says Facebook ROI = 4.0, but Incrementality shows only 1.5x lift.

**Possible explanations:**
1. **Correlation ≠ Causation:** Facebook ads run when people were already going to buy
2. **Halo Effect:** Facebook builds brand awareness that helps other channels
3. **Data Issues:** One method has measurement problems

**What to do:**
- Investigate the discrepancy
- Run additional tests
- Weight the more reliable method higher

---

## Data Transformations {#data-transformations}

### Why Transform Data?

Marketing doesn't work instantaneously or linearly. We need transformations to model reality:

1. **Adstock:** Ads have lingering effects (you remember an ad after seeing it)
2. **Saturation:** Diminishing returns (10th ad impression matters less than 1st)

### Adstock Transformations

#### What is Adstock?

When you see an ad today, it doesn't just affect you today - it sticks in your memory.

**Example:**
- Day 1: You see a Nike ad
- Day 2: Still thinking about Nike (90% memory)
- Day 3: Nike fading (80% memory)
- Day 7: Almost forgotten (30% memory)
- Day 14: Nike who? (5% memory)

#### Geometric Adstock (Most Common)

**Formula:** `Adstocked[t] = Spend[t] + θ × Adstocked[t-1]`

**The θ (theta) parameter:**
- θ = 0.9 → Slow decay (TV, Brand campaigns)
- θ = 0.7 → Medium decay (Display, Social)
- θ = 0.3 → Fast decay (Search, Email)

**Visual Example:**
```
Original Spend:     ████████░░░░░░░░ (spike then nothing)
Adstocked (θ=0.7):  ████████████████░░░░░░░░ (gradual fadeout)
```

#### Weibull Adstock (Advanced)

Allows for delayed peak effect - the ad impact might build up before decaying.

**When to use:** For brand campaigns that take time to build awareness.

### Saturation Transformations

#### What is Saturation?

The more you spend on a channel, the less effective each additional dollar becomes.

**Real-world example:**
- First $10,000 on Facebook: Reaches new people, $50,000 in sales
- Next $10,000: Reaches same people again, $30,000 in sales
- Next $10,000: Ad fatigue sets in, $10,000 in sales

#### Hill Saturation (Most Common)

**The curve looks like an "S" or a bend.**

**Parameters:**
- **Alpha (α):** The spending level where you're at 50% saturation
- **Gamma (γ):** How sharp the curve is
  - γ < 1: Gentle diminishing returns
  - γ > 1: Sharp S-curve (threshold effect)

**Visual:**
```
Response
    │          ___________
    │        /
    │      /
    │    /
    │  /
    │_/__________________ Spend
       ↑
    50% point (Alpha)
```

---

## Budget Optimization {#budget-optimization}

### What is Budget Optimization?

Once you know how each channel performs (from MMM), you can mathematically find the best way to allocate your budget.

### How It Works

1. **Input:** Current budget, channel performance curves
2. **Goal:** Maximize total sales/conversions
3. **Constraints:** Min/max spend per channel, total budget
4. **Output:** Optimal allocation

### Example

**Current allocation:**
- TV: $100,000
- Google: $50,000
- Facebook: $50,000
- Total: $200,000

**MMM Response Curves show:**
- TV is saturated (diminishing returns)
- Google has room to grow
- Facebook is underperforming

**Optimized allocation:**
- TV: $70,000 (-30%)
- Google: $90,000 (+80%)
- Facebook: $40,000 (-20%)
- Total: $200,000

**Result:** Same budget, 15% more sales

### Types of Optimization

| Type | Goal |
|------|------|
| **Maximize Revenue** | Get the most sales |
| **Maximize ROI** | Get best return per dollar |
| **Maximize Reach** | Touch the most people |
| **Minimize CPA** | Lowest cost per acquisition |

---

## Glossary of Terms {#glossary}

### General Terms

| Term | Definition |
|------|------------|
| **Attribution** | Assigning credit for conversions to marketing touchpoints |
| **Baseline** | Sales that would happen with zero marketing |
| **Carryover** | The lingering effect of advertising over time |
| **Contribution** | The portion of sales driven by a specific channel |
| **Conversion** | A desired action (purchase, signup, etc.) |
| **Incrementality** | The true additional impact of marketing |
| **Lift** | The percentage increase caused by a campaign |
| **ROI** | Return on Investment = Revenue / Spend |
| **ROAS** | Return on Ad Spend = Revenue from Ads / Ad Spend |
| **Touchpoint** | Any interaction between customer and brand |

### Statistical Terms

| Term | Definition |
|------|------------|
| **Coefficient** | The multiplier showing a variable's impact |
| **Confidence Interval** | Range of likely true values |
| **p-value** | Probability result is due to chance (<0.05 = significant) |
| **R² Score** | How well the model explains variation (0-1) |
| **Standard Error** | Measure of uncertainty in an estimate |

### MMM Terms

| Term | Definition |
|------|------------|
| **Adstock** | Transformation to model ad memory decay |
| **Control Variable** | Factor that affects sales but isn't marketing (weather, holidays) |
| **Media Variable** | Marketing spending channel |
| **Saturation** | Diminishing returns at high spending levels |

### MTA Terms

| Term | Definition |
|------|------------|
| **Customer Journey** | Sequence of touchpoints before conversion |
| **Lookback Window** | How far back to consider touchpoints |
| **Path** | The specific sequence of channels a customer used |
| **Transition Probability** | Likelihood of moving from one channel to another |

### Incrementality Terms

| Term | Definition |
|------|------------|
| **Control Group** | Group not exposed to the campaign |
| **Holdout** | Portion of audience excluded from ads |
| **Test Group** | Group exposed to the campaign |
| **Treatment Effect** | The measured impact of the campaign |

---

## Quick Reference: When to Use What

| I want to... | Use... |
|--------------|--------|
| Plan annual budgets | MMM |
| Optimize digital campaigns | MTA |
| Prove a campaign works | Incrementality |
| Get the full picture | Triangulation |
| Allocate budget optimally | MMM + Optimization |
| Understand customer paths | MTA (Markov) |
| Measure TV/Radio impact | MMM or Geo Experiments |
| Compare channel efficiency | MMM (ROI) |

---

## Getting Started Checklist

### For MMM:
- [ ] At least 2 years of weekly data (recommended)
- [ ] Sales/conversion data by time period
- [ ] Marketing spend by channel by time period
- [ ] Control variables (promotions, seasonality, etc.)

### For MTA:
- [ ] User-level journey data
- [ ] Timestamp of each touchpoint
- [ ] Channel of each touchpoint
- [ ] Conversion indicator
- [ ] User/Session ID

### For Incrementality:
- [ ] Ability to run controlled experiments
- [ ] Geographic or audience segmentation capability
- [ ] Sufficient sample size
- [ ] Clear success metrics

---

# 📘 PART 1.5: COMPLETE WORKED EXAMPLE

This section walks through a complete example using a fictional company **"ShopMax"** - an online retailer. We'll demonstrate every component with actual numbers, formulas, and step-by-step calculations.

---

## Example Company Profile: ShopMax

**Business:** Online electronics retailer
**Weekly Revenue:** ~$500,000
**Marketing Channels:**
- TV Advertising
- Google Ads (Search + Display)
- Facebook/Instagram Ads
- Email Marketing

**Goal:** Understand which channels drive the most sales and optimize the $200,000 weekly marketing budget.

---

## SECTION A: Marketing Mix Modeling (MMM) - Worked Example

### A.1 Input Data

Here's a sample of ShopMax's weekly data (8 weeks shown, typically need 100+ weeks):

| Week | Date | Sales ($) | TV Spend | Google Spend | Facebook Spend | Email Spend | Promotion | Holiday |
|------|------|-----------|----------|--------------|----------------|-------------|-----------|---------|
| 1 | Jan 1 | 450,000 | 50,000 | 40,000 | 25,000 | 5,000 | 0 | 1 |
| 2 | Jan 8 | 380,000 | 45,000 | 35,000 | 20,000 | 5,000 | 0 | 0 |
| 3 | Jan 15 | 420,000 | 55,000 | 38,000 | 22,000 | 5,000 | 1 | 0 |
| 4 | Jan 22 | 395,000 | 48,000 | 36,000 | 21,000 | 5,000 | 0 | 0 |
| 5 | Jan 29 | 410,000 | 52,000 | 42,000 | 24,000 | 6,000 | 0 | 0 |
| 6 | Feb 5 | 440,000 | 58,000 | 45,000 | 28,000 | 6,000 | 1 | 0 |
| 7 | Feb 12 | 425,000 | 50,000 | 40,000 | 25,000 | 5,500 | 0 | 0 |
| 8 | Feb 19 | 460,000 | 60,000 | 48,000 | 30,000 | 7,000 | 1 | 0 |

### A.2 Step 1: Apply Adstock Transformation

**Why?** TV ads seen today still influence purchases next week. We need to capture this "carryover" effect.

**Formula (Geometric Adstock):**
$$Adstock_t = Spend_t + \theta \times Adstock_{t-1}$$

**Parameters chosen:**
- TV: θ = 0.7 (long-lasting brand effect)
- Google: θ = 0.4 (shorter digital effect)
- Facebook: θ = 0.5 (medium effect)
- Email: θ = 0.2 (very short effect)

**Calculation for TV (θ = 0.7):**

| Week | TV Spend | Calculation | TV Adstock |
|------|----------|-------------|------------|
| 1 | 50,000 | 50,000 + 0.7 × 0 | **50,000** |
| 2 | 45,000 | 45,000 + 0.7 × 50,000 | **80,000** |
| 3 | 55,000 | 55,000 + 0.7 × 80,000 | **111,000** |
| 4 | 48,000 | 48,000 + 0.7 × 111,000 | **125,700** |
| 5 | 52,000 | 52,000 + 0.7 × 125,700 | **139,990** |
| 6 | 58,000 | 58,000 + 0.7 × 139,990 | **155,993** |
| 7 | 50,000 | 50,000 + 0.7 × 155,993 | **159,195** |
| 8 | 60,000 | 60,000 + 0.7 × 159,195 | **171,437** |

**Interpretation:** By Week 8, the effective "advertising pressure" from TV is $171,437, not just $60,000, because previous weeks' ads are still working.

### A.3 Step 2: Apply Saturation Transformation

**Why?** The 10th TV ad someone sees is less impactful than the 1st. Diminishing returns.

**Formula (Hill Saturation):**
$$Saturated = \frac{Adstock^\gamma}{\alpha^\gamma + Adstock^\gamma}$$

**Parameters:**
- α (half-saturation) = 100,000
- γ (shape) = 0.8

**Calculation for Week 8 TV:**
$$Saturated = \frac{171,437^{0.8}}{100,000^{0.8} + 171,437^{0.8}}$$

Step by step:
1. $171,437^{0.8} = 171,437^{0.8} = 35,891$
2. $100,000^{0.8} = 100,000^{0.8} = 25,119$
3. $Saturated = \frac{35,891}{25,119 + 35,891} = \frac{35,891}{61,010} = 0.588$

**Result:** The effective media impact is 0.588 (on a 0-1 scale).

### A.4 Step 3: Run Ridge Regression

**Model Formula:**
$$Sales_t = \beta_0 + \beta_{TV} \cdot TV_{sat,t} + \beta_{Google} \cdot Google_{sat,t} + \beta_{FB} \cdot FB_{sat,t} + \beta_{Email} \cdot Email_{sat,t} + \beta_{Promo} \cdot Promo_t + \beta_{Holiday} \cdot Holiday_t + \epsilon_t$$

**Ridge Regression adds penalty term (λ = 10):**
$$\min \sum(Sales - \hat{Sales})^2 + 10 \times (\beta_{TV}^2 + \beta_{Google}^2 + \beta_{FB}^2 + \beta_{Email}^2)$$

### A.5 Output: Fitted Coefficients

After running the regression on 104 weeks of data:

| Variable | Coefficient (β) | Interpretation |
|----------|-----------------|----------------|
| **Intercept (β₀)** | 180,000 | Baseline weekly sales with zero marketing |
| **TV** | 250,000 | At full saturation, TV adds $250K to sales |
| **Google** | 320,000 | At full saturation, Google adds $320K |
| **Facebook** | 150,000 | At full saturation, Facebook adds $150K |
| **Email** | 80,000 | At full saturation, Email adds $80K |
| **Promotion** | 45,000 | Promotions add $45K |
| **Holiday** | 35,000 | Holidays add $35K |

### A.6 Calculate Channel Contributions

**Formula:**
$$Contribution_{channel,t} = \beta_{channel} \times Saturated_{channel,t}$$

**Week 8 Example:**

| Channel | β (Coef) | Saturated Value | Contribution |
|---------|----------|-----------------|--------------|
| TV | 250,000 | 0.588 | 250,000 × 0.588 = **$147,000** |
| Google | 320,000 | 0.512 | 320,000 × 0.512 = **$163,840** |
| Facebook | 150,000 | 0.445 | 150,000 × 0.445 = **$66,750** |
| Email | 80,000 | 0.385 | 80,000 × 0.385 = **$30,800** |
| Promo | 45,000 | 1 | 45,000 × 1 = **$45,000** |
| Holiday | 35,000 | 0 | 35,000 × 0 = **$0** |
| **Base** | 180,000 | - | **$180,000** |
| **TOTAL** | - | - | **$633,390** |

**Actual Sales Week 8:** $460,000

Note: Predicted is higher than actual, indicating some noise/error (normal).

### A.7 Calculate ROI

**Formula:**
$$ROI_{channel} = \frac{Total\ Contribution_{channel}}{Total\ Spend_{channel}}$$

**Annual Results (52 weeks):**

| Channel | Total Spend | Total Contribution | ROI |
|---------|-------------|-------------------|-----|
| TV | $2,600,000 | $6,500,000 | **2.50** |
| Google | $2,080,000 | $7,280,000 | **3.50** |
| Facebook | $1,196,000 | $2,990,000 | **2.50** |
| Email | $286,000 | $1,144,000 | **4.00** |

### A.8 MMM Output Summary

```
═══════════════════════════════════════════════════════════
                    MMM RESULTS - ShopMax
═══════════════════════════════════════════════════════════

Model Performance:
  • R² Score:        0.87 (87% of sales variance explained)
  • MAPE:            6.2% (average prediction error)
  • Durbin-Watson:   1.92 (no autocorrelation)

Channel Rankings by ROI:
  1. Email:      4.00x  ← Most efficient
  2. Google:     3.50x
  3. TV:         2.50x
  4. Facebook:   2.50x  ← Least efficient

Contribution Share:
  • Base Sales:   35%  ($9.1M annually)
  • TV:           25%  ($6.5M)
  • Google:       28%  ($7.3M)
  • Facebook:     12%  ($2.9M)
  • Email:         4%  ($1.1M)
  • Promotions:    6%  ($1.5M)

═══════════════════════════════════════════════════════════
```

---

## SECTION B: Multi-Touch Attribution (MTA) - Worked Example

### B.1 Input Data

ShopMax tracks customer journeys. Here's a sample:

| User ID | Journey (Touchpoints) | Converted | Revenue |
|---------|----------------------|-----------|---------|
| U001 | Google → Facebook → Email → **Purchase** | Yes | $150 |
| U002 | TV → Google → Google → Facebook → **Purchase** | Yes | $320 |
| U003 | Facebook → Facebook → (no purchase) | No | $0 |
| U004 | Email → Google → **Purchase** | Yes | $85 |
| U005 | Google → TV → Facebook → Google → **Purchase** | Yes | $210 |
| U006 | Facebook → (no purchase) | No | $0 |
| U007 | TV → Google → Email → **Purchase** | Yes | $175 |
| U008 | Google → **Purchase** | Yes | $95 |

### B.2 First Touch Attribution

**Rule:** 100% credit to first touchpoint.

**Calculation for U001 (Revenue = $150):**
- Journey: Google → Facebook → Email → Purchase
- First Touch: **Google**
- Attribution: Google gets $150, others get $0

**All Users Aggregated:**

| Channel | Credit |
|---------|--------|
| Google | $150 + $95 + $210 = **$455** |
| TV | $320 + $175 = **$495** |
| Email | $85 |
| Facebook | $0 |

### B.3 Last Touch Attribution

**Rule:** 100% credit to last touchpoint before conversion.

**Calculation for U001 (Revenue = $150):**
- Journey: Google → Facebook → Email → Purchase
- Last Touch: **Email**
- Attribution: Email gets $150, others get $0

**All Users Aggregated:**

| Channel | Credit |
|---------|--------|
| Email | $150 + $175 = **$325** |
| Facebook | $320 |
| Google | $95 + $210 + $85 = **$390** |
| TV | $0 |

### B.4 Linear Attribution

**Rule:** Equal credit to all touchpoints.

**Calculation for U001 (Revenue = $150):**
- Journey: Google → Facebook → Email (3 touchpoints)
- Credit each: $150 ÷ 3 = **$50 each**

| Channel | Credit from U001 |
|---------|------------------|
| Google | $50 |
| Facebook | $50 |
| Email | $50 |

**Formula:**
$$Credit_{channel} = \frac{Revenue}{n_{touchpoints}}$$

### B.5 Time Decay Attribution

**Rule:** Touchpoints closer to conversion get more credit.

**Formula:**
$$Weight_i = 2^{(i-n)/\lambda}$$

Where:
- i = position (1 to n)
- n = total touchpoints
- λ = half-life (we use 2)

**Calculation for U001 (Revenue = $150, λ = 2):**
- Journey: Google (i=1) → Facebook (i=2) → Email (i=3)
- n = 3

| Position | Channel | Weight Calculation | Weight | Normalized | Credit |
|----------|---------|-------------------|--------|------------|--------|
| 1 | Google | $2^{(1-3)/2} = 2^{-1} = 0.5$ | 0.500 | 0.286 | $42.86 |
| 2 | Facebook | $2^{(2-3)/2} = 2^{-0.5} = 0.707$ | 0.707 | 0.404 | $60.61 |
| 3 | Email | $2^{(3-3)/2} = 2^{0} = 1$ | 1.000 | 0.571 | $85.71 |
| **Sum** | - | - | 1.75 | 1.000 | **$150** |

### B.6 Position-Based (U-Shaped) Attribution

**Rule:** First and last get 40% each, middle shares 20%.

**Calculation for U001 (Revenue = $150):**
- Journey: Google → Facebook → Email
- First (Google): 40% = $60
- Middle (Facebook): 20% = $30
- Last (Email): 40% = $60

**Formula:**
$$Credit_{first} = 0.4 \times Revenue$$
$$Credit_{last} = 0.4 \times Revenue$$
$$Credit_{middle,each} = \frac{0.2 \times Revenue}{n-2}$$

### B.7 Markov Chain Attribution

**Step 1: Build Transition Matrix**

From all customer journeys, count transitions:

| From → To | Start | Google | Facebook | TV | Email | Conv | Null |
|-----------|-------|--------|----------|-----|-------|------|------|
| **Start** | - | 45% | 30% | 20% | 5% | 0% | 0% |
| **Google** | - | 15% | 25% | 10% | 20% | 25% | 5% |
| **Facebook** | - | 20% | 10% | 5% | 15% | 20% | 30% |
| **TV** | - | 35% | 20% | 5% | 10% | 15% | 15% |
| **Email** | - | 10% | 5% | 0% | 5% | 70% | 10% |

**Step 2: Calculate Baseline Conversion Rate**

Using Markov chain simulation (or matrix algebra), calculate:
$$P(Conversion) = 0.35 = 35\%$$

**Step 3: Calculate Removal Effect**

Remove Google from the matrix (redirect to Null):

| From → To | Start | ~~Google~~ | Facebook | TV | Email | Conv | Null |
|-----------|-------|------------|----------|-----|-------|------|------|
| **Start** | - | 0% | 30% | 20% | 5% | 0% | **45%** |

Recalculate conversion rate without Google:
$$P(Conversion | no\ Google) = 0.18 = 18\%$$

**Removal Effect:**
$$RE_{Google} = 1 - \frac{0.18}{0.35} = 1 - 0.514 = 0.486 = 48.6\%$$

**Interpretation:** Removing Google reduces conversions by 48.6%.

**Step 4: Repeat for All Channels**

| Channel | P(Conv) without | Removal Effect |
|---------|-----------------|----------------|
| Google | 18% | 48.6% |
| Facebook | 28% | 20.0% |
| TV | 30% | 14.3% |
| Email | 25% | 28.6% |
| **Sum** | - | 111.5% |

**Step 5: Normalize to Get Attribution**

$$Attribution_{Google} = \frac{48.6\%}{111.5\%} = 43.6\%$$

| Channel | Removal Effect | Attribution |
|---------|----------------|-------------|
| Google | 48.6% | **43.6%** |
| Email | 28.6% | **25.6%** |
| Facebook | 20.0% | **17.9%** |
| TV | 14.3% | **12.8%** |

### B.8 Shapley Value Attribution

**Step 1: Define All Coalitions**

For 4 channels, there are $2^4 = 16$ possible coalitions (including empty set):

| Coalition | Channels | Conversions (v) |
|-----------|----------|-----------------|
| {} | None | 0 |
| {G} | Google only | 150 |
| {F} | Facebook only | 80 |
| {T} | TV only | 60 |
| {E} | Email only | 40 |
| {G,F} | Google + Facebook | 280 |
| {G,T} | Google + TV | 250 |
| {G,E} | Google + Email | 320 |
| {F,T} | Facebook + TV | 160 |
| {F,E} | Facebook + Email | 200 |
| {T,E} | TV + Email | 140 |
| {G,F,T} | Google + FB + TV | 380 |
| {G,F,E} | Google + FB + Email | 450 |
| {G,T,E} | Google + TV + Email | 420 |
| {F,T,E} | FB + TV + Email | 280 |
| {G,F,T,E} | All | 500 |

**Step 2: Calculate Shapley Value for Google**

**Formula:**
$$\phi_G = \sum_{S \subseteq N \setminus \{G\}} \frac{|S|! (n-|S|-1)!}{n!} [v(S \cup \{G\}) - v(S)]$$

For n=4 channels:

| Coalition S | |S| | Weight | v(S) | v(S∪{G}) | Marginal |
|-------------|-----|--------|------|----------|----------|
| {} | 0 | 1/4 | 0 | 150 | 150 |
| {F} | 1 | 1/12 | 80 | 280 | 200 |
| {T} | 1 | 1/12 | 60 | 250 | 190 |
| {E} | 1 | 1/12 | 40 | 320 | 280 |
| {F,T} | 2 | 1/12 | 160 | 380 | 220 |
| {F,E} | 2 | 1/12 | 200 | 450 | 250 |
| {T,E} | 2 | 1/12 | 140 | 420 | 280 |
| {F,T,E} | 3 | 1/4 | 280 | 500 | 220 |

**Shapley Value for Google:**
$$\phi_G = \frac{1}{4}(150) + \frac{1}{12}(200+190+280) + \frac{1}{12}(220+250+280) + \frac{1}{4}(220)$$
$$\phi_G = 37.5 + 55.83 + 62.5 + 55 = 210.83$$

**Repeat for all channels:**

| Channel | Shapley Value | Attribution % |
|---------|---------------|---------------|
| Google | 210.83 | **42.2%** |
| Email | 115.83 | **23.2%** |
| Facebook | 98.33 | **19.7%** |
| TV | 75.00 | **15.0%** |
| **Total** | 500 | 100% |

### B.9 MTA Output Comparison

```
═══════════════════════════════════════════════════════════
              MTA RESULTS COMPARISON - ShopMax
═══════════════════════════════════════════════════════════

                  Google   Facebook    TV      Email
                  ──────   ────────   ────    ─────
First Touch:       44%       0%       48%       8%
Last Touch:        38%      31%        0%      31%
Linear:            35%      22%       18%      25%
Time Decay:        38%      18%       12%      32%
Position-Based:    36%      16%       20%      28%
Markov Chain:      44%      18%       13%      26%  ← Data-driven
Shapley Value:     42%      20%       15%      23%  ← Fair value

RECOMMENDATION:
→ Use Markov/Shapley for strategic decisions
→ Google is consistently the top performer (40-44%)
→ Email punches above its weight (23-32% attribution vs 4% of spend)
═══════════════════════════════════════════════════════════
```

---

## SECTION C: Incrementality Testing - Worked Example

### C.1 Geo Experiment Setup

**Objective:** Prove that Facebook ads actually cause incremental sales (not just correlation).

**Experiment Design:**
- **Test Markets:** New York, Los Angeles, Chicago, Houston, Phoenix (5 cities)
- **Control Markets:** Philadelphia, San Antonio, San Diego, Dallas, San Jose (5 cities)
- **Pre-Period:** 4 weeks (baseline measurement)
- **Treatment Period:** 4 weeks (Facebook ads only in test cities)
- **Metric:** Weekly sales per city

### C.2 Input Data

**Pre-Period (Weeks 1-4):**

| City | Group | Week 1 | Week 2 | Week 3 | Week 4 | Avg |
|------|-------|--------|--------|--------|--------|-----|
| New York | Test | $48,000 | $52,000 | $49,000 | $51,000 | $50,000 |
| Los Angeles | Test | $42,000 | $45,000 | $43,000 | $44,000 | $43,500 |
| Chicago | Test | $38,000 | $40,000 | $39,000 | $41,000 | $39,500 |
| Houston | Test | $32,000 | $34,000 | $33,000 | $35,000 | $33,500 |
| Phoenix | Test | $28,000 | $30,000 | $29,000 | $31,000 | $29,500 |
| **Test Avg** | - | - | - | - | - | **$39,200** |
| Philadelphia | Control | $35,000 | $37,000 | $36,000 | $38,000 | $36,500 |
| San Antonio | Control | $30,000 | $32,000 | $31,000 | $33,000 | $31,500 |
| San Diego | Control | $33,000 | $35,000 | $34,000 | $36,000 | $34,500 |
| Dallas | Control | $36,000 | $38,000 | $37,000 | $39,000 | $37,500 |
| San Jose | Control | $40,000 | $42,000 | $41,000 | $43,000 | $41,500 |
| **Control Avg** | - | - | - | - | - | **$36,300** |

**Post-Period (Weeks 5-8) - Facebook Ads Running in Test Cities:**

| City | Group | Week 5 | Week 6 | Week 7 | Week 8 | Avg |
|------|-------|--------|--------|--------|--------|-----|
| New York | Test | $56,000 | $58,000 | $57,000 | $59,000 | $57,500 |
| Los Angeles | Test | $50,000 | $52,000 | $51,000 | $53,000 | $51,500 |
| Chicago | Test | $45,000 | $47,000 | $46,000 | $48,000 | $46,500 |
| Houston | Test | $38,000 | $40,000 | $39,000 | $41,000 | $39,500 |
| Phoenix | Test | $34,000 | $36,000 | $35,000 | $37,000 | $35,500 |
| **Test Avg** | - | - | - | - | - | **$46,100** |
| Philadelphia | Control | $36,000 | $38,000 | $37,000 | $39,000 | $37,500 |
| San Antonio | Control | $31,000 | $33,000 | $32,000 | $34,000 | $32,500 |
| San Diego | Control | $34,000 | $36,000 | $35,000 | $37,000 | $35,500 |
| Dallas | Control | $37,000 | $39,000 | $38,000 | $40,000 | $38,500 |
| San Jose | Control | $41,000 | $43,000 | $42,000 | $44,000 | $42,500 |
| **Control Avg** | - | - | - | - | - | **$37,300** |

### C.3 Difference-in-Differences Calculation

**The DiD Formula:**
$$\hat{\tau}_{DiD} = (\bar{Y}_{T,post} - \bar{Y}_{T,pre}) - (\bar{Y}_{C,post} - \bar{Y}_{C,pre})$$

**Step-by-Step:**

| Component | Calculation | Value |
|-----------|-------------|-------|
| Test Post Average | $\bar{Y}_{T,post}$ | $46,100 |
| Test Pre Average | $\bar{Y}_{T,pre}$ | $39,200 |
| **Test Change** | $46,100 - 39,200$ | **+$6,900** |
| Control Post Average | $\bar{Y}_{C,post}$ | $37,300 |
| Control Pre Average | $\bar{Y}_{C,pre}$ | $36,300 |
| **Control Change** | $37,300 - 36,300$ | **+$1,000** |
| **DiD Estimate** | $6,900 - 1,000$ | **+$5,900** |

**Interpretation:** Facebook ads caused an incremental $5,900 per city per week.

### C.4 Calculate Lift and Statistical Significance

**Lift Calculation:**
$$Lift = \frac{DiD\ Effect}{Counterfactual} = \frac{5,900}{39,200 + 1,000} = \frac{5,900}{40,200} = 14.7\%$$

**Statistical Test (Two-Sample t-test):**

Test group changes: +$7,500, +$8,000, +$7,000, +$6,000, +$6,000
Control group changes: +$1,000, +$1,000, +$1,000, +$1,000, +$1,000

$$t = \frac{\bar{X}_{test} - \bar{X}_{control}}{\sqrt{\frac{s^2_{test}}{n_{test}} + \frac{s^2_{control}}{n_{control}}}}$$

$$t = \frac{6,900 - 1,000}{\sqrt{\frac{632,500}{5} + \frac{0}{5}}} = \frac{5,900}{355.8} = 16.58$$

**p-value:** < 0.0001 (highly significant)

### C.5 Calculate Incremental ROI (iROAS)

**Campaign Spend:**
- Facebook spend in test cities: $25,000/week × 4 weeks = $100,000 total
- 5 test cities

**Incremental Revenue:**
- Incremental per city per week: $5,900
- Total: $5,900 × 5 cities × 4 weeks = $118,000

**iROAS Calculation:**
$$iROAS = \frac{Incremental\ Revenue}{Campaign\ Spend} = \frac{\$118,000}{\$100,000} = 1.18$$

**Interpretation:** Every $1 spent on Facebook generated $1.18 in incremental revenue.

### C.6 Incrementality Output Summary

```
═══════════════════════════════════════════════════════════
           INCREMENTALITY RESULTS - Facebook Geo Test
═══════════════════════════════════════════════════════════

Experiment Details:
  • Test Markets:     5 cities
  • Control Markets:  5 cities
  • Test Duration:    4 weeks
  • Campaign Spend:   $100,000

Results:
  ┌─────────────────────────────────────────────────────┐
  │  INCREMENTAL LIFT:  +14.7%                          │
  │  p-value:           <0.0001 (***)                   │
  │  Confidence:        99.99%                          │
  └─────────────────────────────────────────────────────┘

Financial Impact:
  • Incremental Revenue:     $118,000
  • Campaign Spend:          $100,000
  • iROAS:                   1.18x
  • Net Incremental Profit:  $18,000

Parallel Trends Check: ✓ PASSED
  Pre-period trends were parallel (p=0.82)

CONCLUSION:
  → Facebook ads CAUSED a 14.7% lift in sales
  → This is not correlation - it's proven causation
  → The campaign was profitable (iROAS > 1.0)
═══════════════════════════════════════════════════════════
```

---

## SECTION D: Triangulation - Combining All Results

### D.1 Compare Results Across Methods

| Channel | MMM ROI | MMM Share | MTA (Markov) | Incrementality iROAS |
|---------|---------|-----------|--------------|---------------------|
| Google | 3.50x | 28% | 44% | - (not tested) |
| Facebook | 2.50x | 12% | 18% | 1.18x |
| TV | 2.50x | 25% | 13% | - (not tested) |
| Email | 4.00x | 4% | 26% | - (not tested) |

### D.2 Identify Discrepancies

**Facebook Analysis:**
- MMM says: ROI = 2.50x
- MTA says: 18% of attribution
- Incrementality says: iROAS = 1.18x

**Why the difference?**

| Method | Measures | Facebook Finding | Explanation |
|--------|----------|------------------|-------------|
| MMM | Historical correlation | ROI 2.50x | Includes halo effects, brand lift |
| MTA | Digital journey credit | 18% | Only counts tracked touchpoints |
| Incrementality | Causal impact | 1.18x | Pure incremental effect |

**Reconciliation:**
- MMM's 2.50x includes long-term brand effects not captured in 4-week test
- True incremental short-term ROI is 1.18x
- Recommended: Weight incrementality higher for Facebook budget decisions

### D.3 Triangulated Recommendations

```
═══════════════════════════════════════════════════════════
              TRIANGULATED INSIGHTS - ShopMax
═══════════════════════════════════════════════════════════

CHANNEL RANKINGS (Confidence-Weighted):

  1. GOOGLE     ★★★★★  High Confidence
     ├─ MMM ROI:      3.50x
     ├─ MTA Share:    44% (highest)
     └─ Action:       INCREASE spend by 20%

  2. EMAIL      ★★★★☆  High Confidence
     ├─ MMM ROI:      4.00x (highest)
     ├─ MTA Share:    26%
     └─ Action:       INCREASE spend by 30%

  3. TV         ★★★☆☆  Medium Confidence
     ├─ MMM ROI:      2.50x
     ├─ MTA Share:    13% (undervalued - offline)
     └─ Action:       MAINTAIN current spend
     └─ Note:         Run geo experiment to validate

  4. FACEBOOK   ★★☆☆☆  Methods Disagree
     ├─ MMM ROI:      2.50x
     ├─ MTA Share:    18%
     ├─ Incr iROAS:   1.18x (validated)
     └─ Action:       DECREASE spend by 15%
     └─ Note:         Incrementality shows lower true impact

OPTIMIZED BUDGET ALLOCATION:
  Current → Recommended

  Google:    $40,000 → $48,000  (+20%)
  Email:     $5,000  → $6,500   (+30%)
  TV:        $50,000 → $50,000  (no change)
  Facebook:  $25,000 → $21,250  (-15%)
  ─────────────────────────────────────
  Total:     $120,000 → $125,750

Expected Impact:
  • Additional Revenue:  +$45,000/week
  • ROI Improvement:     +12%
═══════════════════════════════════════════════════════════
```

---

*Document created for the Marketing Analytics Platform*
*Last updated: December 2025*

This section provides the mathematical formulations, statistical justifications, and technical implementation details for data scientists and technical stakeholders.

---

## 10. Mathematical Foundations of MMM

### 10.1 Ridge Regression - Technical Details

#### The Model Formulation

Ridge Regression is a form of regularized linear regression that adds an L2 penalty to prevent overfitting.

**Standard Linear Regression:**
$$Y = X\beta + \epsilon$$

Where:
- $Y$ = Response variable (sales/conversions) of dimension $n \times 1$
- $X$ = Design matrix of predictors (media spends, controls) of dimension $n \times p$
- $\beta$ = Coefficient vector of dimension $p \times 1$
- $\epsilon$ = Error term, assumed $\epsilon \sim N(0, \sigma^2 I)$

**Ridge Regression Objective Function:**
$$\hat{\beta}_{ridge} = \arg\min_{\beta} \left\{ \sum_{i=1}^{n}(y_i - x_i^T\beta)^2 + \lambda \sum_{j=1}^{p}\beta_j^2 \right\}$$

**Closed-Form Solution:**
$$\hat{\beta}_{ridge} = (X^TX + \lambda I)^{-1}X^TY$$

#### Why Ridge Over OLS?

| Issue with OLS | How Ridge Solves It |
|----------------|---------------------|
| Multicollinearity between media channels | λ penalty shrinks correlated coefficients |
| Overfitting with limited data | Regularization reduces variance |
| Unstable coefficient estimates | Bias-variance tradeoff improves stability |
| Singular $X^TX$ matrix | $(X^TX + \lambda I)$ is always invertible |

#### Choosing λ (Regularization Parameter)

We use **k-fold cross-validation** to select optimal λ:

```python
# Cross-validation grid
λ_values = [0.001, 0.01, 0.1, 1, 10, 100, 1000]

# For each λ:
#   1. Split data into k folds
#   2. Train on k-1 folds, validate on 1
#   3. Calculate RMSE
#   4. Select λ with lowest average RMSE
```

#### Coefficient Interpretation

For a coefficient $\beta_j$ on media channel $j$:
- **Marginal Effect:** A 1-unit increase in $X_j$ leads to $\beta_j$ unit increase in $Y$
- **ROI Calculation:** $ROI_j = \frac{\sum_t \beta_j \cdot X_{jt}}{\sum_t X_{jt}} = \beta_j$ (for linear models without transformations)

### 10.2 Bayesian MMM - Technical Details

#### The Bayesian Framework

**Bayes' Theorem:**
$$P(\theta|D) = \frac{P(D|\theta) \cdot P(\theta)}{P(D)}$$

Where:
- $P(\theta|D)$ = Posterior distribution (what we want)
- $P(D|\theta)$ = Likelihood (how well parameters explain data)
- $P(\theta)$ = Prior distribution (our beliefs before seeing data)
- $P(D)$ = Evidence (normalizing constant)

#### Model Specification

**Likelihood:**
$$Y_t \sim Normal(\mu_t, \sigma^2)$$

$$\mu_t = \alpha + \sum_{j=1}^{J} \beta_j \cdot f(X_{jt}) + \sum_{k=1}^{K} \gamma_k \cdot Z_{kt}$$

Where:
- $\alpha$ = Baseline/intercept
- $\beta_j$ = Media coefficient for channel $j$
- $f(X_{jt})$ = Transformed media (with adstock/saturation)
- $\gamma_k$ = Control variable coefficients
- $Z_{kt}$ = Control variables (seasonality, trends, etc.)

#### Prior Distributions (Default)

```python
# Priors used in our implementation
α ~ Normal(μ_y, σ_y)           # Intercept centered on mean of Y
β_j ~ HalfNormal(σ=10)         # Media coefficients (positive)
γ_k ~ Normal(0, 5)             # Control coefficients
σ ~ HalfCauchy(β=1)            # Error standard deviation
```

**Justification for Priors:**
| Prior | Justification |
|-------|---------------|
| HalfNormal for media | Media should have positive effect (or zero) |
| Wide σ=10 | Weakly informative, lets data speak |
| HalfCauchy for σ | Heavy tails handle outliers, recommended by Gelman |

#### MCMC Sampling (NUTS Algorithm)

We use the **No-U-Turn Sampler (NUTS)**, an adaptive variant of Hamiltonian Monte Carlo:

1. **Initialization:** Start from random point in parameter space
2. **Warm-up/Tuning:** Adapt step size and mass matrix (1000 iterations)
3. **Sampling:** Draw from posterior (2000 iterations × 4 chains)
4. **Diagnostics:** Check $\hat{R}$ < 1.01, ESS > 400

**Convergence Diagnostics:**
- **$\hat{R}$ (R-hat):** Should be < 1.01 for all parameters
- **ESS (Effective Sample Size):** Should be > 400
- **Trace Plots:** Should show good mixing (caterpillar pattern)
- **Divergences:** Should be 0 (indicates sampling problems)

#### Posterior Inference

**Point Estimates:**
- Mean: $\hat{\beta}_j = \frac{1}{S}\sum_{s=1}^{S}\beta_j^{(s)}$
- Median: 50th percentile of samples

**Credible Intervals:**
- 95% CI: [2.5th percentile, 97.5th percentile]
- HDI (Highest Density Interval): Narrowest interval containing 95% of mass

### 10.3 Multiplicative (Log-Log) Model

#### Model Formulation

**Multiplicative Model:**
$$Y = \alpha \cdot \prod_{j=1}^{J} X_j^{\beta_j} \cdot \epsilon$$

**Log-Linearized Form:**
$$\log(Y) = \log(\alpha) + \sum_{j=1}^{J} \beta_j \cdot \log(X_j) + \log(\epsilon)$$

#### Interpretation as Elasticity

The coefficient $\beta_j$ represents **elasticity**:
$$\beta_j = \frac{\partial \log Y}{\partial \log X_j} = \frac{\partial Y / Y}{\partial X_j / X_j}$$

**Interpretation:** A 1% increase in $X_j$ leads to $\beta_j$% change in $Y$.

| Elasticity Value | Interpretation |
|------------------|----------------|
| $\beta_j$ = 0.1 | 10% increase in spend → 1% increase in sales |
| $\beta_j$ = 0.5 | 10% increase in spend → 5% increase in sales |
| $\beta_j$ > 1 | Increasing returns (rare, usually early stage) |
| $\beta_j$ < 1 | Diminishing returns (typical) |

#### Why Use Multiplicative Model?

1. **Natural saturation:** $X^{0.3}$ automatically exhibits diminishing returns
2. **Multiplicative interactions:** Channels naturally interact
3. **Proportional effects:** Percentage-based interpretation
4. **Handles scale differences:** Log transformation normalizes scales

---

## 11. Mathematical Foundations of MTA

### 11.1 Heuristic Models - Formal Definitions

Let $J = \{c_1, c_2, ..., c_n\}$ be a customer journey with $n$ touchpoints.

#### First Touch Attribution
$$A(c_i) = \begin{cases} 1 & \text{if } i = 1 \\ 0 & \text{otherwise} \end{cases}$$

#### Last Touch Attribution
$$A(c_i) = \begin{cases} 1 & \text{if } i = n \\ 0 & \text{otherwise} \end{cases}$$

#### Linear Attribution
$$A(c_i) = \frac{1}{n} \quad \forall i \in \{1, ..., n\}$$

#### Time Decay Attribution
$$A(c_i) = \frac{2^{(i-n)/\lambda}}{\sum_{j=1}^{n} 2^{(j-n)/\lambda}}$$

Where $\lambda$ = half-life parameter (default = 7 days)

#### Position-Based (U-Shaped) Attribution
$$A(c_i) = \begin{cases} 
0.4 & \text{if } i = 1 \\
0.4 & \text{if } i = n \\
\frac{0.2}{n-2} & \text{otherwise}
\end{cases}$$

### 11.2 Markov Chain Attribution - Technical Details

#### Markov Property

A first-order Markov Chain assumes:
$$P(S_{t+1} | S_t, S_{t-1}, ..., S_1) = P(S_{t+1} | S_t)$$

The future state depends only on the current state, not the history.

#### Transition Probability Matrix

For channels $\{C_1, C_2, ..., C_k, Start, Conversion, Null\}$:

$$P = \begin{bmatrix}
p_{11} & p_{12} & \cdots & p_{1,k+3} \\
p_{21} & p_{22} & \cdots & p_{2,k+3} \\
\vdots & \vdots & \ddots & \vdots \\
p_{k+3,1} & p_{k+3,2} & \cdots & p_{k+3,k+3}
\end{bmatrix}$$

Where $p_{ij} = P(\text{next state} = j | \text{current state} = i)$

**Estimation from data:**
$$\hat{p}_{ij} = \frac{n_{ij}}{\sum_j n_{ij}}$$

Where $n_{ij}$ = count of transitions from state $i$ to state $j$.

#### Removal Effect Calculation

The **removal effect** for channel $C_k$ measures how much conversion probability drops when removing that channel:

1. **Calculate baseline conversion rate:** $P(Conv)$ using full transition matrix
2. **Remove channel $C_k$:** Set $p_{ik} = 0$ and redistribute to Null state
3. **Calculate new conversion rate:** $P(Conv | remove\ C_k)$
4. **Removal Effect:** $RE_k = 1 - \frac{P(Conv | remove\ C_k)}{P(Conv)}$

#### Attribution Calculation

$$Attribution(C_k) = \frac{RE_k}{\sum_{j=1}^{K} RE_j}$$

#### Higher-Order Markov Chains

For more accuracy, we can use second-order Markov chains:
$$P(S_{t+1} | S_t, S_{t-1})$$

This captures two-step patterns like "Google → Facebook → Conversion" being different from "Facebook → Google → Conversion".

### 11.3 Shapley Value Attribution - Technical Details

#### Game Theory Foundation

Based on cooperative game theory. Each channel is a "player" and the "game value" is total conversions.

#### Shapley Value Formula

For player $i$ in game $(N, v)$:
$$\phi_i(v) = \sum_{S \subseteq N \setminus \{i\}} \frac{|S|! (|N|-|S|-1)!}{|N|!} [v(S \cup \{i\}) - v(S)]$$

Where:
- $N$ = set of all channels
- $S$ = subset of channels (coalition)
- $v(S)$ = value (conversions) generated by coalition $S$
- $v(S \cup \{i\}) - v(S)$ = marginal contribution of channel $i$

#### Properties of Shapley Value (Axioms)

| Property | Description | Why It Matters |
|----------|-------------|----------------|
| **Efficiency** | $\sum_i \phi_i = v(N)$ | All credit is distributed |
| **Symmetry** | Equal contributors get equal credit | Fair treatment |
| **Linearity** | $\phi(v+w) = \phi(v) + \phi(w)$ | Consistent across games |
| **Null Player** | Zero contribution → zero credit | No free-riding |

#### Computational Complexity

For $k$ channels: $O(2^k)$ coalitions to evaluate.

**Approximation methods for large $k$:**
1. Monte Carlo sampling of permutations
2. Truncated Shapley (limit coalition sizes)
3. Kernel SHAP (regression-based approximation)

---

## 12. Mathematical Foundations of Incrementality

### 12.1 Geo Experiments - Difference-in-Differences (DiD)

#### The DiD Estimator

$$\hat{\tau}_{DiD} = (\bar{Y}_{T,post} - \bar{Y}_{T,pre}) - (\bar{Y}_{C,post} - \bar{Y}_{C,pre})$$

Where:
- $\bar{Y}_{T,post}$ = Average outcome for Treatment group in post-period
- $\bar{Y}_{T,pre}$ = Average outcome for Treatment group in pre-period
- $\bar{Y}_{C,post}$ = Average outcome for Control group in post-period
- $\bar{Y}_{C,pre}$ = Average outcome for Control group in pre-period

#### Regression Formulation

$$Y_{it} = \alpha + \beta_1 \cdot Treat_i + \beta_2 \cdot Post_t + \beta_3 \cdot (Treat_i \times Post_t) + \epsilon_{it}$$

Where:
- $\beta_3$ = **DiD estimator** (treatment effect)
- $Treat_i$ = 1 if unit $i$ is in treatment group
- $Post_t$ = 1 if time $t$ is in post-period

#### Parallel Trends Assumption

**Critical assumption:** In absence of treatment, treatment and control groups would have followed parallel trends.

$$E[Y_{T,post}^{(0)} - Y_{T,pre}] = E[Y_{C,post} - Y_{C,pre}]$$

**How to validate:**
1. Visual inspection of pre-period trends
2. Placebo tests with fake treatment dates
3. Statistical test for slope differences in pre-period

#### Standard Error Estimation

For clustered data (by geo), use **clustered standard errors**:
$$SE_{clustered} = \sqrt{\frac{G}{G-1} \cdot \frac{N-1}{N-K} \cdot \sum_{g=1}^{G} u_g' u_g}$$

Where $G$ = number of clusters (geos), $u_g$ = residuals for cluster $g$.

### 12.2 Synthetic Control Method

#### Motivation

When only one or few treatment units exist, we construct a "synthetic" control from weighted combination of control units.

#### Optimization Problem

Find weights $W = (w_1, w_2, ..., w_J)$ that minimize:
$$\min_W ||X_1 - X_0 W||^2$$

Subject to:
- $w_j \geq 0$ for all $j$
- $\sum_{j=1}^{J} w_j = 1$

Where:
- $X_1$ = Pre-treatment characteristics of treated unit
- $X_0$ = Pre-treatment characteristics of control units

#### Treatment Effect Estimation

$$\hat{\tau}_t = Y_{1t} - \sum_{j=1}^{J} w_j^* Y_{jt}$$

For each post-treatment period $t$.

#### Inference via Placebo Tests

1. Apply synthetic control to each control unit (pretend it was treated)
2. Calculate placebo effects for all control units
3. Compare actual treatment effect to placebo distribution
4. p-value = fraction of placebos with larger effects

**RMSPE Ratio:**
$$\text{Ratio} = \frac{\text{Post-treatment RMSPE}}{\text{Pre-treatment RMSPE}}$$

### 12.3 Statistical Power Analysis

#### Minimum Detectable Effect (MDE)

$$MDE = (t_{\alpha/2} + t_{\beta}) \cdot \sqrt{\frac{2\sigma^2}{n}}$$

Where:
- $t_{\alpha/2}$ = critical value for significance level (1.96 for 95%)
- $t_{\beta}$ = critical value for power (0.84 for 80% power)
- $\sigma^2$ = variance of outcome
- $n$ = sample size per group

#### Sample Size Calculation

$$n = 2 \cdot \left(\frac{(t_{\alpha/2} + t_{\beta}) \cdot \sigma}{\delta}\right)^2$$

Where $\delta$ = expected effect size.

---

## 13. Data Transformations - Technical Details

### 13.1 Adstock Transformation

#### Geometric Adstock (Recursive)

$$Adstock_t = X_t + \theta \cdot Adstock_{t-1}$$

**Expanded form:**
$$Adstock_t = \sum_{l=0}^{\infty} \theta^l \cdot X_{t-l}$$

The total weight over infinite lags:
$$\sum_{l=0}^{\infty} \theta^l = \frac{1}{1-\theta}$$

**Half-life calculation:**
$$t_{1/2} = \frac{\log(0.5)}{\log(\theta)}$$

| θ Value | Half-life (weeks) | Typical Channel |
|---------|-------------------|-----------------|
| 0.3 | 0.6 | Search, Email |
| 0.5 | 1.0 | Display |
| 0.7 | 1.9 | Social |
| 0.9 | 6.6 | TV, Brand |

#### Weibull Adstock (Delayed Peak)

$$w_l = \frac{shape}{scale} \cdot \left(\frac{l}{scale}\right)^{shape-1} \cdot \exp\left(-\left(\frac{l}{scale}\right)^{shape}\right)$$

**When to use:**
- Brand campaigns with delayed response
- TV campaigns that build awareness over time
- New product launches

### 13.2 Saturation Transformation

#### Hill Function

$$f(x) = \frac{x^\gamma}{\alpha^\gamma + x^\gamma}$$

**Properties:**
- $f(0) = 0$ (no spend → no effect)
- $f(\alpha) = 0.5$ (α is half-saturation point)
- $\lim_{x \to \infty} f(x) = 1$ (maximum response is 1)
- $\gamma$ controls steepness

**Parameterization guide:**
| γ Value | Shape | Interpretation |
|---------|-------|----------------|
| γ < 1 | Concave | Strong diminishing returns from start |
| γ = 1 | Standard | Michaelis-Menten kinetics |
| γ > 1 | S-curve | Threshold effect before response |
| γ > 3 | Step-like | Binary on/off response |

#### Response Curve Calibration

1. **From experiments:** Use incrementality test results to anchor curve
2. **From benchmarks:** Industry studies suggest γ ∈ [0.4, 0.8] for most media
3. **Cross-validation:** Fit model with different α, γ and evaluate MAPE

### 13.3 Combined Transformation

**Full transformation pipeline:**
$$X_{transformed} = Saturation(Adstock(X_{original}))$$

**Order matters:** Apply adstock first (temporal), then saturation (response shape).

---

## 14. Budget Optimization - Technical Details

### 14.1 Optimization Problem Formulation

**Objective:**
$$\max_{x_1, ..., x_k} \sum_{j=1}^{K} \beta_j \cdot f_j(x_j)$$

**Subject to:**
$$\sum_{j=1}^{K} x_j = B \quad \text{(Budget constraint)}$$
$$x_j^{min} \leq x_j \leq x_j^{max} \quad \text{(Channel bounds)}$$
$$x_j \geq 0 \quad \text{(Non-negativity)}$$

Where:
- $x_j$ = spend on channel $j$
- $\beta_j$ = channel coefficient from MMM
- $f_j$ = saturation function for channel $j$
- $B$ = total budget

### 14.2 Solution Methods

#### Gradient-Based (L-BFGS-B)

For smooth saturation curves, use quasi-Newton methods:

```python
from scipy.optimize import minimize

result = minimize(
    fun=lambda x: -total_response(x),  # Negative for maximization
    x0=current_allocation,
    method='L-BFGS-B',
    bounds=[(min_j, max_j) for j in channels],
    constraints={'type': 'eq', 'fun': lambda x: sum(x) - budget}
)
```

#### Marginal ROI Approach

At optimum, marginal ROI should be equal across all channels:
$$\frac{\partial Response_j}{\partial x_j} = \lambda \quad \forall j$$

Where $\lambda$ is the shadow price of the budget constraint.

**Iterative algorithm:**
1. Calculate marginal ROI for each channel at current allocation
2. Move budget from lowest marginal ROI to highest
3. Repeat until marginal ROIs converge

### 14.3 Scenario Analysis

**Monte Carlo Simulation for Uncertainty:**
```python
optimized_allocations = []
for i in range(1000):
    # Sample coefficients from posterior (Bayesian) or bootstrap
    beta_sample = sample_coefficients()
    
    # Optimize with sampled coefficients
    optimal_x = optimize(beta_sample)
    optimized_allocations.append(optimal_x)

# Report: mean allocation and confidence intervals
```

---

## 15. Model Validation & Diagnostics

### 15.1 MMM Validation Metrics

| Metric | Formula | Good Value | Interpretation |
|--------|---------|------------|----------------|
| **R²** | $1 - \frac{SS_{res}}{SS_{tot}}$ | > 0.8 | % variance explained |
| **Adjusted R²** | $1 - \frac{(1-R^2)(n-1)}{n-p-1}$ | > 0.75 | Penalizes over-fitting |
| **MAPE** | $\frac{100}{n}\sum\|\frac{y-\hat{y}}{y}\|$ | < 10% | Average % error |
| **RMSE** | $\sqrt{\frac{1}{n}\sum(y-\hat{y})^2}$ | Context-dependent | Absolute error |
| **DW Statistic** | $\frac{\sum(e_t - e_{t-1})^2}{\sum e_t^2}$ | 1.5 - 2.5 | Autocorrelation check |

### 15.2 Out-of-Sample Validation

**Time-Series Cross-Validation:**
```
Fold 1: Train [----] Test [-]
Fold 2: Train [-----] Test [-]
Fold 3: Train [------] Test [-]
Fold 4: Train [-------] Test [-]
```

Never use future data to predict past (prevents leakage).

### 15.3 Coefficient Sanity Checks

| Check | Expected | Red Flag |
|-------|----------|----------|
| Media coefficients | Positive | Negative (wrong sign) |
| Seasonality | Matches business | Inverted patterns |
| Competitor | Negative | Strongly positive |
| ROI range | 0.5 - 5.0 | > 10 or < 0 |

---

## 16. Technical Implementation Notes

### 16.1 Handling Missing Data

**Strategies implemented:**
1. **Time gaps:** Linear interpolation for < 10% missing
2. **Media zeros:** Keep as-is (valid data point)
3. **Imputation:** Forward-fill for sporadic missing values

### 16.2 Multicollinearity Detection

**Variance Inflation Factor (VIF):**
$$VIF_j = \frac{1}{1 - R_j^2}$$

Where $R_j^2$ = R² from regressing $X_j$ on all other predictors.

| VIF Value | Interpretation |
|-----------|----------------|
| 1 | No correlation |
| 1-5 | Moderate (acceptable) |
| 5-10 | High (concerning) |
| > 10 | Severe (action needed) |

**Solutions:**
1. Ridge regression (already implemented)
2. Principal Component Regression
3. Remove highly correlated variables

### 16.3 Code Architecture

```
src/
├── models/
│   ├── mmm/
│   │   ├── base_mmm.py      # Abstract base class
│   │   ├── ridge_mmm.py     # Ridge implementation
│   │   ├── bayesian_mmm.py  # PyMC implementation
│   │   └── multiplicative_mmm.py
│   └── mta/
│       ├── heuristic.py     # Rule-based models
│       ├── markov_mta.py    # Markov Chain
│       └── shapley_mta.py   # Game theory
├── incrementality/
│   ├── geo_experiment.py    # DiD analysis
│   ├── synthetic_control.py # Synthetic control
│   └── holdout_test.py      # Randomized tests
└── transformations/
    ├── adstock.py           # Carryover effects
    └── saturation.py        # Diminishing returns
```

---

## 17. References & Further Reading

### Academic Papers

1. **MMM Foundations:**
   - Dekimpe, M. G., & Hanssens, D. M. (1999). "Sustained spending and persistent response: A new look at long-term marketing profitability." *Journal of Marketing Research.*

2. **Bayesian MMM:**
   - Jin, Y., et al. (2017). "Bayesian Methods for Media Mix Modeling with Carryover and Shape Effects." *Google Research.*

3. **Markov Attribution:**
   - Anderl, E., et al. (2016). "Mapping the customer journey: Lessons learned from graph-based online attribution modeling." *International Journal of Research in Marketing.*

4. **Shapley Value:**
   - Shapley, L. S. (1953). "A value for n-person games." *Contributions to the Theory of Games.*

5. **Synthetic Control:**
   - Abadie, A., & Gardeazabal, J. (2003). "The economic costs of conflict: A case study of the Basque Country." *American Economic Review.*

### Industry Resources

- Google's MMM Framework (Meridian)
- Meta's Robyn (Open-source MMM)
- Nielsen Marketing Mix Modeling
- Measured.com Incrementality Framework

---

*Technical documentation for the Marketing Analytics Platform*
*Last updated: December 2025*
