# Article Plan: "The Square of the Reverse Is the Reverse of the Square"

A working plan for a short, publishable popular-math article built around this
observation:

```
12² = 144      21² = 441
13² = 169      31² = 961
```

Reverse the number, and the square reverses too. The article's job is to make
the reader feel the surprise, then show them the one idea that explains it
completely, then hand them tools to explore further.

---

## 1. Goal and audience

- **Deliverable:** one article, roughly 1,200 to 1,800 words, with 3 to 4 small
  figures or tables. Long enough to explain the "why", short enough to read in
  one sitting.
- **Audience:** curious general readers and students. No prerequisite beyond
  knowing how long multiplication works. Anyone who has ever done a
  multiplication by hand has already met the key idea (carrying).
- **Venue options (pick one before drafting, since it sets tone and length):**
  - Personal blog / Substack / Medium: most freedom, can include code.
  - Recreational math outlets (e.g. a Math Horizons style piece, Chalkdust,
    Plus Magazine): tighter word limit, expect a small "for the reader" puzzle.
  - Aeon-style long form is overkill for this topic.
- **Tone:** playful and concrete. Lead with numbers, not with definitions.

## 2. The mathematical core (verified)

These facts were checked by brute force up to 100,000 and should anchor the
article. Every claim below is safe to print.

**The rule.** Write n with digits d₁d₂…dₖ. Squaring it by long multiplication
produces "column sums" c₀, c₁, …, c₂ₖ₋₂ where cₘ = Σ dᵢ·dⱼ over all i + j = m.
If every column sum is at most 9, no carrying happens, and the digits of n²
are literally those column sums, in order. Reversing n reverses the list of
digits, which reverses the list of column sums (the formula is symmetric in
i and j). So:

> **rev(n²) = (rev n)² exactly when squaring n produces no carries.**

Brute force confirms this is an "if and only if" with zero exceptions below
100,000 (trailing zeros excluded, since reversing 120 gives 21, not 021).

**Why the digits are always small.** A digit d contributes d² to some column
sum. Since 4² = 16 > 9, any number containing a 4 or larger will carry, so
every working number uses only the digits 0, 1, 2, 3. Even a 3 is fragile:
33 fails because 3·3 + 3·3 = 18 carries.

**The complete list of working pairs below 100,000** (66 pairs; the smaller
member is shown, and palindromes are excluded since they trivially work):

| Digits | Count | Examples |
|--------|-------|----------|
| 2 | 2 | 12, 13 |
| 3 | 5 | 102, 103, 112, 113, 122 |
| 4 | 18 | 1002, 1011, 1012, 1021, 1102, 1112, 1122, 1212, 2012, 2022, … |
| 5 | 41 | 10002, 10111, 10212, 11112, 11121, 12012, … |

**A satisfying near miss for the article:** 14² = 196 but 41² = 1681. Column
sums for 14 are 1, 8, 16. The 16 carries and the symmetry is destroyed.

**Bonus facts to sprinkle in:**
- Palindromes with palindromic squares are the same phenomenon: 11² = 121,
  111² = 12321, 1111² = 1234321. The famous "1234321" pattern is the no-carry
  rule in disguise, and it breaks at 1111111111² because a column sum
  reaches 10.
- The cube version is much rarer: below 100,000, only 1011 and 10011 satisfy
  rev(n³) = (rev n)³. Cubing has three-way column sums, so carries are harder
  to avoid.
- 13 and 31 are both prime, and 12 and 21 share the same digit sum. These are
  charming but do not explain anything, and the article should say so
  explicitly to avoid the reader chasing numerology.
- The rule is not special to base 10. In base b, the digits must keep every
  column sum below b. Larger bases allow more working numbers; base 2 allows
  almost none. This is a good "one more idea" for the closing section.

## 3. Article structure

Working title options (decide during editing):
- *The Square of the Reverse*
- *144, 441, and the Secret of Not Carrying*
- *Why 12 and 21 Are Mirror Images All the Way Down*

### Section 1: The hook (150 words)
Open with the two pairs, displayed large. Ask the reader to try 14 and 41
themselves and watch it fail. The failure is the real hook: the pattern is
not universal, so something specific must be going on.

### Section 2: Hunting for more (250 words)
Present the small working numbers: 12, 13, 102, 103, 112, 113, 122. Invite the
reader to notice what they have in common (the digits never exceed 3, and
mostly they are 0s and 1s). Do not explain yet. Table or short list.

### Section 3: Long multiplication, revisited (400 words)
This is the heart. Show 12 × 12 done by hand, column by column, and then
21 × 21 next to it. Point out that the columns are the same numbers in
reverse order. Then show 14 × 14 and highlight the moment the 16 carries.
State the rule in plain words: reversal survives squaring exactly when
nothing carries.

A good figure here: the two long multiplications side by side with columns
color coded, and the "carry" in the 14 case circled.

### Section 4: Why the carry is the whole story (250 words)
Give the slightly more formal argument with the column-sum formula. Keep it
to one displayed equation. Make the symmetry (swapping i and j) the punchline:
reversing the digits reverses the columns because multiplication does not care
about order.

### Section 5: Consequences and curiosities (300 words)
- No digit above 3 can ever appear. Why.
- The 121, 12321, 1234321 family is the same fact.
- The cube version and its two lonely examples.
- One sentence on other bases.
- One sentence dismissing the "13 and 31 are both prime" coincidence.

### Section 6: Try it yourself (150 words)
Give the reader a challenge: find the smallest 6-digit working number, or
find a working number containing two 3s. Include the ten-line program in an
appendix or a footnote for readers who want to search on their own.

## 4. Figures and tables

1. **Hero image:** 12² = 144 and 21² = 441 stacked with a mirror line between
   them. Same for 13 and 31.
2. **Side-by-side long multiplication:** 12 × 12 versus 21 × 21, columns
   colored to show the reversal.
3. **The failure:** 14 × 14 with the carry highlighted.
4. **Table:** count of working numbers by digit length (from Section 2 above).

All four can be produced as plain text or simple SVG. No plotting library
needed.

## 5. Verification script

Keep this in the repo alongside the article so every number printed can be
regenerated. It is also the "try it yourself" appendix.

```python
def rev(n):
    return int(str(n)[::-1])

def works(n):
    return n % 10 != 0 and rev(n * n) == rev(n) ** 2

def no_carry(n):
    d = [int(c) for c in str(n)]
    cols = [0] * (2 * len(d) - 1)
    for i, a in enumerate(d):
        for j, b in enumerate(d):
            cols[i + j] += a * b
    return max(cols) <= 9

hits = [n for n in range(1, 100_000) if works(n) and n < rev(n)]
print(len(hits), hits[:20])
assert all(works(n) == no_carry(n) for n in range(1, 100_000) if n % 10)
```

## 6. Writing steps

1. **Choose the venue** (sets the word budget and whether code is allowed).
2. **Draft Sections 1 to 3 first.** If the long-multiplication explanation
   does not land in draft, the article does not work; fix that before writing
   the rest.
3. **Draft Sections 4 to 6.**
4. **Build the four figures.** Text or SVG, keep them small.
5. **Fact-check pass:** rerun the script, confirm every number in the text
   appears in its output. Check the 1111111111² claim by hand or code.
6. **Read-aloud edit** for tone. Cut any sentence that defines something the
   reader does not need.
7. **Get one non-mathematician to read it** and ask them to explain the rule
   back. If they cannot, rework Section 3.
8. **Publish**, and include the script link or appendix.

## 7. Things to avoid

- Do not open with a definition of "digit reversal". Open with 144 and 441.
- Do not claim the property is rare or common without a number attached.
- Do not present the prime coincidence (13, 31) as meaningful.
- Do not use the word "theorem" before Section 4. Let the reader discover it.
- Do not include trailing-zero cases (120, 021) without explaining the
  convention; simplest is to exclude them in one sentence.

## 8. Optional extensions (only if the venue wants more)

- A short section on **general bases**, with a table of how many 3-digit
  working numbers exist in bases 2 through 16.
- The **product version**: when is rev(a·b) = rev(a)·rev(b)? Same no-carry
  idea, e.g. 12 × 13 = 156 and 21 × 31 = 651.
- A sidebar on **Kaprekar-style digit games** to place this in a family of
  similar curiosities.
