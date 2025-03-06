# Bonding Curve vs. Conventional Crypto Pricing

This document explains, with formulas, the difference between the bonding curve model used in this contract and the typical pricing mechanism used in Automated Market Makers (AMMs) like Uniswap.

---

## 1. Bonding Curve Model (Based on Contract Formulas)

### Token Issuance Function

In our contract, the total token issuance as a function of the contributed ETH is given by:

$$
S(x) = T - \frac{k}{x_0 + x}
$$

where:
- $T$ is the total "virtual" number of tokens available for issuance.
- $k$ is a scaling constant for ETH units.
- $x_0$ is the virtual initial ETH deposit (e.g., 2 ETH).
- $x$ is the additional real ETH contributed.

This function means that as more ETH is contributed, the term $\frac{k}{x_0 + x}$ decreases, and thus $S(x)$ (the cumulative number of tokens issued) increases.

### Inverse Bonding Curve Function

To determine how much additional ETH $x$ is needed to reach a desired token issuance $S$, we invert the above function:

$$
x = \frac{k}{T - S} - x_0
$$

This expression calculates the required ETH contribution to achieve a total issuance of $S$.

### Instantaneous Token Price

The instantaneous price per token is derived as the inverse of the derivative of $S(x)$ with respect to $x$:

$$
p(x) = \left(\frac{dS}{dx}\right)^{-1} = \frac{(x_0 + x)^2}{k}
$$

This means that the price increases quadratically with the total contributed ETH ($x_0 + x$).

---

## 2. Conventional Crypto Pricing (AMM Model)

In traditional AMMs like Uniswap, pricing is based on the constant product formula:

$$
x \cdot y = k'
$$

where:
- $x$ and $y$ are the reserves of the two tokens in the liquidity pool.
- $k'$ is a constant invariant.

The price is determined by the ratio of the reserves:

$$
p = \frac{y}{x}
$$

In this model, each trade shifts the reserves $x$ and $y$, and the price dynamically adjusts to reflect current supply and demand.

---

## Key Differences

### Deterministic vs. Market-Driven Pricing

**Bonding Curve:**  
  The price is determined by a predetermined function:
  
  $$
  p(x) = \frac{(x_0 + x)^2}{k}
  $$
  
  It depends solely on the cumulative ETH contributed and follows a fixed, predictable formula.

**AMM:**  
  The price is driven by market dynamics and is given by:
  
  $$
  p = \frac{y}{x}
  $$
  
  It changes as trades occur and the pool's reserves adjust.

### Predictability

- **Bonding Curve:**  
  The pricing mechanism is fully deterministic. Every additional ETH increases the token supply and price in a known manner.

- **AMM:**  
  The price is subject to external market forces. Trades alter the reserve ratios, making the price less predictable and more reflective of real-time demand and supply.

### Incentivization

- **Bonding Curve:**  
  The early phase offers lower prices (when $x$ is small), incentivizing early investors. As more funds are contributed, the price increases according to the formula.

- **AMM:**  
  The pricing is set by current pool reserves without an inherent mechanism for early investor incentives. Price fluctuations depend on market activity rather than a preset formula.

---

## Conclusion

In summary, the bonding curve model in our contract is defined by:

**Token Issuance Function:**
  
  $$
  S(x) = T - \frac{k}{x_0 + x}
  $$

**Inverse Function:**
  
  $$
  x = \frac{k}{T - S} - x_0
  $$

**Instantaneous Price:**
  
  $$
  p(x) = \frac{(x_0 + x)^2}{k}
  $$

This deterministic model provides predictable, formula-based pricing that benefits early investors by starting at a lower price and gradually increasing as more ETH is added. In contrast, conventional AMM pricing is based on the dynamic ratio of token reserves ($p = \frac{y}{x}$), adjusting continuously with market trades.

This document aims to clarify the fundamental differences between these two approaches to crypto pricing.
