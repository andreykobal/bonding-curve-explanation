# Bonding Curve for Token Sale

## Abstract

This section outlines the mathematical model of the bonding curve used for dynamic token pricing during the token sale. The model enables early investors to acquire tokens at a lower price, with the price increasing as more ETH is contributed. The formulas below describe the mechanism for token issuance, the inverse calculation of the required ETH contribution, and the determination of the instantaneous token price.

## Parameters and Constants

The model uses the following constants defined in the smart contract:

- **\(T\)** — Total “virtual” number of tokens available for issuance.  
  **Value:**
  $$
  T = 1\,073\,000\,191 \times 10^{18}
  $$

- **\(k\)** — A positive constant that scales the dependency on the contributed ETH.  
  **Value:**
  $$
  k = 2\,146\,000\,382 \times 10^{18}
  $$

- **\(x_0\)** — The virtual initial ETH deposit used for the initial adjustment of the curve.  
  **Value:**
  $$
  x_0 = 2 \times 10^{18} \quad (\text{equivalent to 2 ETH})
  $$

Additional parameters include:
- **\(\text{FEE\_BPS} = 100\)** (a 1% fee),
- **\(\text{BPS\_DENOMINATOR} = 10\,000\)**,
- **Sale Token Cap:** 800,000,000 tokens available for sale.

## 1. Token Issuance Function

The token issuance is defined by the function:

$$
S(x) = T - \frac{k}{x_0 + x}
$$

where:
- \( S(x) \) is the total number of tokens issued when the cumulative ETH contribution is \( x_0 + x \);
- \( x \) is the additional ETH contributed by users (beyond the base \( x_0 \)).

**Explanation:**

- **Initial State (\(x = 0\)):**  
  $$
  S(0) = T - \frac{k}{x_0}
  $$
  Even without any additional contribution, there is a base issuance determined by \( T \) and \( x_0 \).

- **As \(x\) Increases:**  
  The total contribution \( x_0 + x \) increases, which decreases the fraction \( \frac{k}{x_0 + x} \) and, consequently, increases \( S(x) \). In other words, the token issuance grows as more ETH is contributed.

## 2. Inverse Bonding Curve Function

To determine the required ETH contribution \( x \) for a desired token issuance \( S \), we start with the equation:

$$
S = T - \frac{k}{x_0 + x}
$$

**Step-by-step solution:**

1. Rearrange the equation:
   $$
   \frac{k}{x_0 + x} = T - S
   $$

2. Solve for \( x_0 + x \):
   $$
   x_0 + x = \frac{k}{T - S}
   $$

3. Isolate \( x \):
   $$
   x = \frac{k}{T - S} - x_0
   $$

**Interpretation:**  
This formula calculates the additional ETH required to reach a total token issuance of \( S \).

## 3. Instantaneous Token Price

The instantaneous token price \( p(x) \) reflects the cost in ETH to purchase one additional token when \( x \) ETH has already been contributed. It is derived by differentiating the token issuance function \( S(x) \).

**Calculation of the derivative:**

Given:
$$
S(x) = T - \frac{k}{x_0 + x}
$$

Differentiate with respect to \( x \):
$$
\frac{dS}{dx} = \frac{k}{(x_0 + x)^2}
$$

The instantaneous price is the inverse of this derivative:
$$
p(x) = \left(\frac{dS}{dx}\right)^{-1} = \frac{(x_0 + x)^2}{k}
$$

**Explanation:**  
This shows that the cost per token increases quadratically with the total contributed ETH \( (x_0 + x) \). In other words, as more ETH is added, each additional token costs more.

## Conclusion

The mathematical model of the bonding curve in the smart contract is defined by the following key relationships:

1. **Token Issuance Function:**
   $$
   S(x) = T - \frac{k}{x_0 + x}
   $$
   Token issuance increases as additional ETH is contributed.

2. **Inverse Function:**
   $$
   x = \frac{k}{T - S} - x_0
   $$
   This equation determines the required ETH for achieving a specified token issuance \( S \).

3. **Instantaneous Token Price:**
   $$
   p(x) = \frac{(x_0 + x)^2}{k}
   $$
   The cost to purchase an additional token increases quadratically with the total ETH contributed.

This model implements an automated and dynamic pricing mechanism that encourages early investment while progressively increasing the token price as more funds are raised. The additional parameters, such as the 1% fee and the sale cap of 800 million tokens, are integrated to ensure a robust and fair token sale process.
