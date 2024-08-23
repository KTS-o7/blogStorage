+++
title = 'Problem 592 Fraction Addition and Subtraction'
date = 2024-08-23T12:53:21+05:30
draft = false
series = 'leetcode'
tags =['string','math','regex']
toc = false
math = false
+++

# Problem Statement

**Link** - [Problem 592](https://leetcode.com/problems/fraction-addition-and-subtraction/description/)

## Question

Given a string `expression` representing an expression of fraction addition and subtraction, return the calculation result in string format.

The final result should be an irreducible fraction. If your final result is an integer, change it to the format of a fraction that has a denominator `1`. So in this case, `2` should be converted to `2/1`.

#### [Irreducible fraction](https://en.wikipedia.org/wiki/Irreducible_fraction)

## Example 1

```
Input: expression = "-1/2+1/2"
Output: "0/1"
```

## Example 2

```
Input: expression = "-1/2+1/2+1/3"
Output: "1/3"
```

## Example 3

```
Input: expression = "1/3-1/2"
Output: "-1/6"
```

## Constraints

```markdown
- The input string only contains `'0'` to `'9'`, `'/'`, `'+'` and `'-'`. So does the output.
- Each fraction (input and output) has the format `±numerator/denominator`.
  If the first input fraction or the output is positive, then `'+'` will be omitted.
- The input only contains valid irreducible fractions,
  where the numerator and denominator of each fraction will always be in the range `[1, 10]`.
  If the denominator is `1`, it means this fraction is actually an integer in a fraction format defined above.
- The number of given fractions will be in the range `[1, 10]`.
- The numerator and denominator of the final result are guaranteed to be valid and in the range of 32-bit int.
```

## Solution

```cpp
class Solution {
public:
    string fractionAddition(string exp) {
        int numer = 0, denom = 1;
        int idx = 0;
        int size = exp.size();

        while(idx<size){
            int sign = 1;
            if(exp[idx] == '+' || exp[idx] == '-'){
                if(exp[idx]=='-')
                    sign = -1;
                idx++;
            }

            int num = 0;
            while(idx<size && isdigit(exp[idx])){
                num = num*10 + (exp[idx]-'0');
                idx++;
            }

            num*=sign;
            idx++;

            int den = 0;
             while(idx<size && isdigit(exp[idx])){
                den = den*10 + (exp[idx]-'0');
                idx++;
            }

            numer = numer*den + num*denom;
            denom = denom*den;

            int GCD = gcd(abs(numer),denom);
            numer /= GCD;
            denom /= GCD;
        }
        return to_string(numer)+'/'+to_string(denom);
    }
};
```

## Complexity Analysis

```markdown
| Algorithm | Time Complexity | Space Complexity |
| --------- | --------------- | ---------------- |
| Traversal | O(n)            | O(1)             |
```

## Explanation

### 1. Intuition

```markdown
- We will use the normal logic of adding fractions.
- Two fractions can be added by taking the LCM of the denominators and then adding the numerators.
- that is `a/b + c/d = (a*d + b*c)/(b*d)`
- We can make the fraction irreducible by dividing the numerator and denominator by their GCD.
- We will keep on adding the fractions and then at the end we will return the final fraction.
```

### 2. Implementation

```markdown
- Initialize `numer` and `denom` to `0` and `1` respectively.
- Initialize `idx` to `0` and `size` to the size of the input string.
- While `idx` is less than `size` do :
  - Initialize `sign` to `1`.
  - If the character at `idx` is `'+'` or `'-'` then do the following:
    - If the character is `'-'` then set `sign` to `-1`.
    - Increment `idx`.
  - Initialize `num` to `0`. This is the numerator of the fraction.
  - While `idx` is less than `size` and the character at `idx` is a digit do the following:
    - Multiply `num` by `10` and add the digit at `idx` to `num`.
    - Increment `idx`.
  - Multiply `num` by `sign`.
  - Increment `idx`.
  - Initialize `den` to `0`. This is the denominator of the fraction.
  - While `idx` is less than `size` and the character at `idx` is a digit do the following:
    - Multiply `den` by `10` and add the digit at `idx` to `den`.
    - Increment `idx`.
  - Update `numer` and `denom` using the formula:
    - `numer = numer * den + num * denom`.
    - `denom = denom * den`.
  - Calculate the GCD of `abs(numer)` and `denom`.
  - Divide `numer` and `denom` by the GCD.
- Return the final fraction as a string.
```
