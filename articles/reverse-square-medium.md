# The Square of the Reverse

*What 144 and 441 know about long multiplication that you forgot*

---

Here are two multiplications. Look at them for a moment before reading on.

```
12² = 144
21² = 441
```

Flip the number, and the square flips with it. That could be a fluke, so try another pair:

```
13² = 169
31² = 961
```

Same trick. At this point a reasonable person starts to suspect a law. Reverse a number, square it, and you get the reverse of the original square. It feels like the kind of thing that ought to be always true, in the way that reversing a word and then reversing it again gives the word back.

So try 14.

```
14² = 196
41² = 1681
```

It falls apart immediately. Reversing 196 gives 691, which is not 1681, and is not even the right number of digits. Whatever is going on with 12 and 13, it is not a universal law. It is something more specific, and more interesting, than that.

## Hunting for more

The natural next move is to go looking for other numbers that work. I wrote a ten-line program (it is at the end of this article) and asked it for every number below 100,000 with this mirror property. I threw out palindromes like 11 and 121, which reverse to themselves and so work for a boring reason, and I threw out numbers ending in zero, since reversing 120 gives 021 and nobody wants to argue about leading zeros.

Here are the smallest survivors:

```
12,  13,
102, 103, 112, 113, 122,
1002, 1003, 1011, 1012, 1013, 1021, 1022, 1031, ...
```

Take a second to look at the digits. Not the numbers, the digits.

There are no 4s. No 5s, 6s, 7s, 8s, or 9s. The largest digit that ever appears is 3, and even 3 shows up rarely and only once per number. Almost everything is made of 0s, 1s, and 2s. Below 100,000 there are exactly 66 working pairs, and every single one of them obeys this digit diet.

That is a strong hint. Whatever breaks 14 has something to do with the 4 being too big. But too big for what?

## Long multiplication, revisited

You learned to multiply multi-digit numbers by hand sometime around age nine, and you have probably not thought carefully about it since. Let us do 12 × 12 slowly, and instead of the usual layout, let us organize it by columns.

Write 12 as the digits **1** and **2**. When you square it, every digit of the top number gets multiplied by every digit of the bottom number, and each product lands in a column determined by where the two digits sat:

```
          1     2
        × 1     2
        -----------
   column:  hundreds   tens   ones
   pieces:   1·1      1·2     2·2
                      2·1
   sums:      1        4       4
```

The ones column gets 2·2 = 4. The tens column gets 1·2 and 2·1, which add to 4. The hundreds column gets 1·1 = 1. Read the column sums left to right: **1, 4, 4**. That is 144.

Now do 21 × 21 the same way:

```
          2     1
        × 2     1
        -----------
   column:  hundreds   tens   ones
   pieces:   2·2      2·1     1·1
                      1·2
   sums:      4        4       1
```

Column sums: **4, 4, 1**. That is 441.

Look at what happened. The column sums for 21 are the column sums for 12 in reverse order. Of course they are. Reversing the digits swaps which digit is "on the left" and which is "on the right," and that swaps which column each product lands in. The middle column is unchanged, because 1·2 and 2·1 are the same thing. Multiplication does not care about order, and that symmetry is the entire secret.

So why does 14 fail? Do the same thing:

```
          1     4
        × 1     4
        -----------
   column:  hundreds   tens   ones
   pieces:   1·1      1·4     4·4
                      4·1
   sums:      1        8      16
```

The column sums are **1, 8, 16**. And there is the problem. A column is only allowed to hold one digit. That 16 has to be split: write down the 6, carry the 1 into the tens column, which becomes 9. The answer is 196, and the clean list 1, 8, 16 has been mangled into 1, 9, 6.

Now reverse: 41 × 41 has column sums **16, 8, 1**. The 16 is at the front this time, where nothing sits to its left, so it simply spills into a new digit. You get 1681. The two answers, 196 and 1681, came from the same three column sums in opposite orders. They only look unrelated because carrying happened in different places.

**Here is the rule in one sentence.** Reversing survives squaring exactly when squaring produces no carries.

If every column sum stays at 9 or below, the column sums *are* the digits of the answer, the reversal of the number reverses the column sums, and the reversal of the square comes out for free. The moment any column sum reaches 10, carrying rewrites the digits in a direction-dependent way and the mirror shatters.

## Why the carry is the whole story

For anyone who wants the argument in general, here it is. Write your number as digits d₁ d₂ … dₖ. When you square it, the column that is m places from the left collects every product dᵢ · dⱼ where i + j lands in that column:

```
column m  =  sum of  dᵢ · dⱼ   over all pairs with  i + j = m
```

Reversing the number sends digit dᵢ to position k + 1 − i. Run that substitution through the formula and the column at position m becomes the column at position 2k + 2 − m. Every column sum moves to the mirror-image column, and none of them change value, because dᵢ · dⱼ = dⱼ · dᵢ.

So the reversed number has exactly the reversed list of column sums. If no column sum exceeds 9, those column sums are digits, and we are done. If some column sum is 10 or more, carrying kicks in, and carrying is the one step in arithmetic that only flows in one direction.

My program checked this against every number below 100,000. Every number whose reversal squares correctly has no carries, and every number with no carries reverses correctly. No exceptions.

## What the rule explains

Once you have the rule, the strange digit diet stops being strange.

**Why no digit above 3.** A digit d sitting anywhere in your number contributes d² to some column. Since 4² = 16 is already too big for one column, any number containing a 4 or higher will carry. So only 0, 1, 2, and 3 are allowed. Even 3 is on thin ice: 33 fails, because the middle column is 3·3 + 3·3 = 18.

**Why 121, 12321, 1234321 look the way they do.** You may have seen this famous pattern:

```
11²   = 121
111²  = 12321
1111² = 1234321
```

It is the same phenomenon. A string of k ones has column sums 1, 2, 3, …, k, …, 3, 2, 1, and as long as k stays at 9 or below, nothing carries and you read the pattern straight off. At ten ones the middle column hits 10, a carry happens, and you get 1234567900987654321. The pyramid collapses exactly where the rule says it should.

**Why cubes are almost hopeless.** You can ask the same question for cubes: when does the reverse of n³ equal the cube of the reverse? The argument is the same, but now each column collects three-way products dᵢ · dⱼ · dₗ, and there are many more of them, so columns overflow much more easily. Below 100,000 there are exactly two working pairs: 1011 with 1101, and 10011 with 11001. Check the first one if you like:

```
1011³ = 1033364331
1101³ = 1334633301
```

**Why 13 and 31 being prime means nothing.** It is tempting to notice that 13 and 31 are both prime, or that 12 and 21 have the same digit sum, and to wonder if that is part of the story. It is not. The rule is about carrying and nothing else. 13 works because 1, 6, 9 are all single digits, and the primality is a coincidence that happens to be pretty.

**Why base 10 is not special.** In base b, the rule reads "no column sum may reach b." In a large base like 16, more digits are small enough to be safe and more numbers work. In base 2, a column sum of 2 already carries, so almost nothing survives. The phenomenon is really about the gap between how big a column can get and how big a digit is allowed to be.

## Try it yourself

Two puzzles, in increasing order of sneakiness.

**Puzzle 1.** What is the smallest 6-digit number with the mirror property? Reason it out before running code. (Hint: what is the smallest 5-digit one, and why?)

**Puzzle 2.** Find a working number that contains two 3s. Take your time on this one. If you get stuck, ask yourself where the two 3s' product lands, and how many times it lands there.

And here is the program, so you can check everything in this article and go hunting for more. It also asserts the main rule, so if it runs without complaint, the theorem holds for every number it checked.

```python
def rev(n):
    return int(str(n)[::-1])

def mirrors(n):
    return n % 10 != 0 and rev(n * n) == rev(n) ** 2

def no_carry(n):
    d = [int(c) for c in str(n)]
    cols = [0] * (2 * len(d) - 1)
    for i, a in enumerate(d):
        for j, b in enumerate(d):
            cols[i + j] += a * b
    return max(cols) <= 9

hits = [n for n in range(1, 100_000) if mirrors(n) and n < rev(n)]
print(len(hits), hits[:20])

# the whole point of the article, as one line
assert all(mirrors(n) == no_carry(n) for n in range(1, 100_000) if n % 10)
```

*Answers: the smallest 6-digit example is 100002, for the same reason 10002 is the smallest 5-digit one: a 1 at the front, a 2 at the back, and zeros keeping them apart. Puzzle 2 has no answer at all. Two 3s at positions i and j always meet in column i + j, contributing 3·3 twice, and 18 carries. No number with two 3s can ever have the mirror property, in any length, and now you can prove it.*
