# IEEE 754
Useful Links:
- [IEEE-754 Wikipedia Page](https://en.wikipedia.org/wiki/IEEE_754)
- [IEEE-754 Converter](https://www.h-schmidt.net/FloatConverter/IEEE754.html)

## Structure

Using 32bit single precision IEEE 754 as an example to store $15.5_{10}$

| Sign (1 bit) | Exponent (8 bits) | Significand/Mantissa (23 bits) |  
|--------------|-------------------|--------------------------------|
| 0            | 10000010          | 11110000000000000000000        |

### Sign
The sign bit is the first bit in the IEEE 754 representation. A value of `0` indicates a positive number, and a value of `1` indicates a negative number. In this case, the sign bit is `0`, indicating that $15.5_{10}$ is positive.

### Exponent
The exponent is stored using a biased representation. The formula for the bias is:

$$
\text{Bias} = 2^{k-1} - 1
$$

Where $k$ is the number of bits used for the exponent field.

- For **single-precision** (32-bit), the exponent field has 8 bits, so the bias is:

$$
\text{Bias} = 2^{8-1} - 1 = 127
$$

- For **double-precision** (64-bit), the exponent field has 11 bits, so the bias is:

$$
\text{Bias} = 2^{11-1} - 1 = 1023
$$

The exponent is stored as the value of the exponent plus the bias. The formula for the actual exponent is:

$$
E = e - \text{bias}
$$

Where:
- $E$ is the actual exponent (i.e., the exponent in base 10),
- $e$ is the exponent stored in the IEEE 754 format (in binary),
- $\text{bias}$ is a constant used to offset the exponent so that both positive and negative exponents can be represented.

In this case, the exponent value is $130$ which is $10000010_2$ in binary, and the actual exponent is calculated as:

$$
130 - 127 = 3
$$

### Significand
The significand (or mantissa) represents the precision bits of the number, excluding the leading 1 (implicit in normalized numbers). The stored value of the significand is `1.1111...` in binary, and the remaining bits are padded with zeros. In this case, the mantissa is `11110000000000000000000`, which corresponds to the fractional part of the number.

## Addition

When adding two IEEE 754 floating-point numbers, the following steps are generally followed:

1. **Align the exponents**: If the exponents of the two numbers are not the same, the smaller exponent is adjusted by shifting its significand to the right.
2. **Add the significands**: After aligning the exponents, the significands (mantissas) are added. If necessary, the result is normalized (shifting the result and adjusting the exponent).
3. **Round the result**: If the result is not exactly representable, rounding must be performed.
4. **Normalize the result**: If the result of the addition is out of range or not normalized, it is adjusted to fit the IEEE 754 format.

## Basic and interchange formats

IEEE 754 specifies two primary formats for storing floating-point numbers: the **basic format** and the **interchange format**.

- **Basic Format**: This is the format in which floating-point numbers are stored in memory. For single precision, this is the 32-bit representation of the number, with 1 bit for the sign, 8 bits for the exponent, and 23 bits for the significand (mantissa).

- **Interchange Format**: This format is used for the exchange of floating-point data between different systems. The key difference is that the exponent is adjusted to represent the number in a normalized form (if applicable) and may use different conventions for handling special cases like infinity and NaN (Not a Number).


## Examples

<tabs>
<tab id="converting_example" title="Decimal to IEEE-754">
<h3>Convert 29.125 to IEEE 754 Single Precision</h3>

<h4>Step 1: Convert to Binary</h4>
<p>To convert 29.125 to binary, we first handle the whole number (29) and the decimal part (0.125). Start by converting 29 to binary through repeated division by 2, recording the remainders:</p>
<p>29 / 2 = 14 remainder <strong>1</strong></p>
<p>14 / 2 = 7 remainder <strong>0</strong></p>
<p>7 / 2 = 3 remainder <strong>1</strong></p>
<p>3 / 2 = 1 remainder <strong>1</strong></p>
<p>1 / 2 = 0 remainder <strong>1</strong></p>
<p>Reading the remainders from bottom to top, we get <strong>11101</strong>.</p>

<p>Next, convert the decimal part (0.125). Multiply the decimal by 2 and record the integer part:</p>
<p>0.125 * 2 = 0.25, integer part <strong>0</strong></p>
<p>0.25 * 2 = 0.5, integer part <strong>0</strong></p>
<p>0.5 * 2 = 1.0, integer part <strong>1</strong>; stop when the result is 1.0</p>
<p>The decimal part in binary is <strong>.001</strong>. Combining it with the whole number, we get <strong>11101.001</strong>.</p>

<h4>Step 2: Determine the Sign</h4>
<p>Since 29.125 is positive, the sign bit is <strong>0</strong>.</p>

<h4>Step 3: Determine the Exponent</h4>
<p>To find the exponent, shift the binary point to normalize the number. For <strong>11101.001</strong>, shift the binary point 4 places to the left, so the exponent is <strong>4</strong>.</p>
<p>IEEE-754 uses a bias of 127 for single precision. So, add 127 to the exponent: <strong>4 + 127 = 131</strong>.</p>
<p>Now, convert 131 to binary by dividing by 2:</p>
<p>131 / 2 = 65 remainder <strong>1</strong></p>
<p>65 / 2 = 32 remainder <strong>1</strong></p>
<p>32 / 2 = 16 remainder <strong>0</strong></p>
<p>16 / 2 = 8 remainder <strong>0</strong></p>
<p>8 / 2 = 4 remainder <strong>0</strong></p>
<p>4 / 2 = 2 remainder <strong>0</strong></p>
<p>2 / 2 = 1 remainder <strong>0</strong></p>
<p>1 / 2 = 0 remainder <strong>1</strong></p>
<p>Reading the remainders from bottom to top, we get <strong>10000011</strong>.</p>

<h4>Step 4: Determine the Mantissa</h4>
<p>For the mantissa, take the binary digits to the right of the binary point in the normalized number <strong>1.1101001</strong>. Remove the leading 1 (which is implied), leaving <strong>1101001</strong>. Then, pad it with zeros to the right to get 23 bits: <strong>11010010000000000000000</strong>.</p>

<h4>Step 5: Putting It All Together</h4>
<p>The final IEEE 754 single precision representation of 29.125 is:</p>
<p><strong>0 | 10000011 | 11010010000000000000000</strong></p>
<a href="http://localhost:63342/com-bufonu-education/preview/ieee-754.html">Verify answer here</a>
</tab>

<tab id="addition_example" title="Addition">
This will be added soon. Maybe tomorrow!
</tab>
</tabs>
