Absolutely — here’s a **system design interview Powers of 10 cheat sheet** you can memorize.

### 1. Numbers you should know

| Power |                 Value | Name        |
| ----- | --------------------: | ----------- |
| 10³   |                 1,000 | Thousand    |
| 10⁶   |             1,000,000 | Million     |
| 10⁹   |         1,000,000,000 | Billion     |
| 10¹²  |     1,000,000,000,000 | Trillion    |
| 10¹⁵  | 1,000,000,000,000,000 | Quadrillion |

**Key conversions:**

- 1 million = **10⁶**
- 1 billion = **10⁹**
- 1 trillion = **10¹²**

---

### 2. Time conversions

For estimation, remember:

- 1 minute ≈ **60 seconds ≈ 10²**
- 1 hour ≈ **3,600 seconds ≈ 10³**
- 1 day ≈ **86,400 seconds ≈ 10⁵**
- 1 month ≈ **2.6 × 10⁶ seconds**
- 1 year ≈ **3.15 × 10⁷ seconds**

The most important one:

> **1 year ≈ 3 × 10⁷ seconds**

---

### 3. Requests → QPS

Use:

**QPS = requests per year ÷ 3 × 10⁷**

|       Requests/year | Approx QPS |
| ------------------: | ---------: |
|     1 million (10⁶) |       0.03 |
|    10 million (10⁷) |        0.3 |
|   100 million (10⁸) |          3 |
| **1 billion (10⁹)** |     **33** |
|   10 billion (10¹⁰) |        333 |
|  100 billion (10¹¹) |      3,333 |
|   1 trillion (10¹²) |     33,333 |

**Interview shortcut:**

> **1 billion requests/year ≈ 30 QPS**

---

### 4. Users → Requests

Suppose you have:

**100 million DAU**

= **10⁸ users/day**

If each user makes **10 requests/day**:

**10⁸ × 10 = 10⁹ requests/day**

Then:

**10⁹ / 10⁵ ≈ 10⁴ QPS**

So roughly:

> **10,000 QPS average**

For peak traffic, you might assume **2–5× average**, depending on the system.

---

### 5. Storage estimation

Memorize:

| Size |     Approx |
| ---- | ---------: |
| 1 KB |  10³ bytes |
| 1 MB |  10⁶ bytes |
| 1 GB |  10⁹ bytes |
| 1 TB | 10¹² bytes |
| 1 PB | 10¹⁵ bytes |

Example:

**100 million users × 1 KB/user**

= 10⁸ × 10³ bytes
= **10¹¹ bytes**
≈ **100 GB**

---

### 6. Bandwidth estimation

Formula:

> **Bandwidth = QPS × request size**

Example:

**10,000 QPS × 10 KB**

= 10⁴ × 10⁴ bytes/sec
= **10⁸ bytes/sec**

≈ **100 MB/s**

In bits:

**100 MB/s × 8 ≈ 800 Mbps**

≈ **0.8 Gbps**

---

### 7. Very useful interview memory table

| Item             | Approximate value |
| ---------------- | ----------------: |
| 1 million        |               10⁶ |
| 1 billion        |               10⁹ |
| 1 trillion       |              10¹² |
| 1 year           |       3 × 10⁷ sec |
| 1 day            |           10⁵ sec |
| 1 GB             |         10⁹ bytes |
| 1 TB             |        10¹² bytes |
| 1 PB             |        10¹⁵ bytes |
| 1 billion/year   |           ~30 QPS |
| 10 billion/year  |          ~300 QPS |
| 100 billion/year |        ~3,000 QPS |
| 1 trillion/year  |       ~30,000 QPS |

### 8. The interview estimation pattern

When asked to **“estimate the scale”**, follow this:

**Users → Actions → Requests → QPS → Data → Storage → Bandwidth**

For example:

> 100M users → 10 actions/day → 1B requests/day → ~10K average QPS → calculate request size → calculate bandwidth → calculate daily/yearly storage.

**The 5 numbers I'd memorize first:**
**10⁶ = million, 10⁹ = billion, 10¹² = trillion, 10⁵ = seconds/day, 3×10⁷ = seconds/year.**
