# Ad Auction Mechanism Simulator: GSP vs. VCG 📉💰

![Python](https://img.shields.io/badge/Python-3.9%2B-blue)
![Library](https://img.shields.io/badge/Library-NumPy%20%7C%20Matplotlib%20%7C%20Seaborn-orange)
![Topic](https://img.shields.io/badge/Topic-Mechanism%20Design%20%7C%20Game%20Theory-green)

## 📌 Executive Summary
This project simulates a **Multi-Slot Real-Time Bidding (RTB)** environment to analyze the economic trade-offs between two dominant auction mechanisms in Ad Tech:
1.  **Generalized Second-Price (GSP):** The industry standard (used by Google, Pinterest, Meta).
2.  **Vickrey-Clarke-Groves (VCG):** The theoretical "truthful" benchmark.

By simulating 1,000+ auction rounds with strategic AI agents, this project demonstrates that while **GSP generates ~7.1% higher revenue** for the platform, it is **not incentive compatible**, creating a complex environment where bidders are mathematically incentivized to "shade" (lower) their bids to maximize utility.

---

## 📊 Key Simulation Results
The simulation compared a **"Truthful" strategy** against a **"Strategic Shading" strategy** (bidding 80% of true value).

![Auction Simulation Results](https://github.com/Kartikay77/auction-mechanism-simulator/blob/main/MEDIA/Auction_515.PNG)

### 1. The Incentive Gap (Why Bidders Lie)
* **VCG Mechanism (Orange):** Strategic shading **never** paid off (0.0% of cases). Truth-telling is a dominant strategy (Nash Equilibrium).
* **GSP Mechanism (Blue):** Strategic shading increased bidder utility in **42.3%** of auctions.
    * *Business Implication:* In a GSP marketplace, advertisers are forced to invest in complex "bid shading" algorithms to avoid overpaying, increasing the technical barrier to entry.

### 2. The Revenue Trade-Off
Despite its flaws, GSP is often preferred by platforms because it extracts more surplus from bidders in competitive environments.
* **Average GSP Revenue:** `$1,497.53`
* **Average VCG Revenue:** `$1,397.55`
* **Result:** GSP generated **7.1% more revenue** than VCG in this simulation setting.

---

## 🧠 Theoretical Background

### Generalized Second-Price (GSP)
* **Rule:** The winner of Slot $k$ pays the bid of the winner of Slot $k+1$.
* **Flaw:** Unlike a single-item second-price auction, GSP is not incentive compatible for multiple slots. A bidder might prefer to lose the top slot to get the 2nd slot at a much lower price, creating instability.

### Vickrey-Clarke-Groves (VCG)
* **Rule:** Bidders pay the "social cost" (externality) their presence imposes on other bidders.
* **Advantage:** Truthful bidding is a dominant strategy. Bidders simply bid their true valuation.
* **Disadvantage:** Lower revenue for the publisher and harder to explain to non-technical advertisers.

---

## 🛠️ Simulation Logic

The `AdAuction` class simulates a marketplace with:
* **Multiple Slots:** 3 Ad positions with decreasing Click-Through Rates (CTRs: 100, 60, 30).
* **Stochastic Demand:** 10 Bidders with private valuations drawn from a uniform distribution $U \sim [1, 10]$.
* **Strategic Agents:**
    * `Truthful_Agent`: Bids $b_i = v_i$
    * `Shading_Agent`: Bids $b_i = \alpha \cdot v_i$ (where $\alpha = 0.8$)

### Code Snippet: GSP Payment Calculation
```python
# In GSP, you pay the bid of the person directly below you
next_bid = sorted_bids[i+1] if (i+1) < len(bids) else 0
payment = next_bid * ctr
