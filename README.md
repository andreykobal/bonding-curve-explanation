# Bonding Curve for Token Sale

## Abstract

This document describes the mathematical model of the bonding curve used for dynamic token pricing during the token sale. The model is designed to reward early investors by offering a lower price at the beginning and gradually increasing the token price as more ETH is contributed. This README outlines the underlying formulas and explains how the model works, along with instructions for viewing the mathematical expressions on GitHub Pages using MathJax.

## Parameters and Constants

The following constants are defined in the smart contract:

- **\( T \)** — Total “virtual” number of tokens available for issuance.  
  **Value:**
  $$
  T = 1\,073\,000\,191 \times 10^{18}
  $$

- **\( k \)** — A positive constant that scales the dependency on the contributed ETH.  
  **Value:**
  $$
  k = 2\,146\,000\,382 \times 10^{18}
  $$

- **\( x_0 \)** — The virtual initial ETH deposit used for the initial adjustment of the curve.  
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
- \( S(x) \) is the total number of tokens issued for a cumulative ETH contribution of \( x_0 + x \);
- \( x \) is the additional ETH contributed beyond the base \( x_0 \).

### Explanation

- **Initial State (\( x = 0 \)):**  
  $$
  S(0) = T - \frac{k}{x_0}
  $$
  Even without any additional contribution, there is a base issuance determined by \( T \) and \( x_0 \).

- **As \( x \) Increases:**  
  The cumulative contribution \( x_0 + x \) increases, which decreases the term \( \frac{k}{x_0 + x} \) and, consequently, increases \( S(x) \). In other words, the token issuance grows as more ETH is contributed.

## 2. Inverse Bonding Curve Function

To determine the required additional ETH \( x \) for a desired token issuance \( S \), we start with the equation:

$$
S = T - \frac{k}{x_0 + x}
$$

### Step-by-Step Solution

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
This formula calculates the additional ETH needed to achieve a total token issuance of \( S \).

## 3. Instantaneous Token Price

The instantaneous token price \( p(x) \) represents the cost in ETH for acquiring one additional token when \( x \) ETH has already been contributed. It is derived by differentiating the token issuance function.

### Derivation

Given:
$$
S(x) = T - \frac{k}{x_0 + x}
$$

Differentiate with respect to \( x \):
$$
\frac{dS}{dx} = \frac{k}{(x_0 + x)^2}
$$

The instantaneous price is the inverse of the derivative:
$$
p(x) = \left(\frac{dS}{dx}\right)^{-1} = \frac{(x_0 + x)^2}{k}
$$

**Explanation:**  
This shows that the cost per token increases quadratically with the cumulative ETH contribution \( (x_0 + x) \). Hence, as more ETH is contributed, each additional token costs more.

## Conclusion

The bonding curve model in the smart contract is defined by three key relationships:

1. **Token Issuance Function:**
   $$
   S(x) = T - \frac{k}{x_0 + x}
   $$
   Token issuance increases as additional ETH is contributed.

2. **Inverse Function:**
   $$
   x = \frac{k}{T - S} - x_0
   $$
   This equation computes the required additional ETH for a specified token issuance \( S \).

3. **Instantaneous Token Price:**
   $$
   p(x) = \frac{(x_0 + x)^2}{k}
   $$
   The price per token increases quadratically with the total ETH contributed.

