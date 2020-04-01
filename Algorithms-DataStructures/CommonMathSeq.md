# Common Sum of Sequences

###### Add explanation where possible

- `2^0 + 2^1 + 2^2 + 2^3 + .... 2^N` === `2^(N + 1) - 1`

- `2^1 + 2^2 + 2^3 + 2^4 + ... + 2^N` === `2^(N + 1)`

- 1 + 2 + 4 + 8 + 16 + ... + X

  - This can also be written as X + X/2 + X/4 + X/8 + ... + 1
  - This is roughly 2X

- `(N - 1) + (N - 2) + (N - 3) + ... + 2 + 1` === `N * (N - 1) / 2`
  - Explanation
  - the LHS is a sum of (N - 1) terms (from 1 to (N - 1) there are (N - 1) items)
  - Now, rearrange, so that after the first, comes the last, after the second comes the second last. Like this `(N - 1) + 1 + (N - 2) + 3 + (N - 3) + 3 ...`
  - Notice `(N - 1) + 1 === N` && `(N - 2) + 2 === N` && so on.
  - Hence all these pairs have a sum of N.
  - Since, originally, there were (N - 1) terms, so now we have `(N - 1) / 2` such pairs.
  - So, the total value is `N * (N - 1) / 2`
