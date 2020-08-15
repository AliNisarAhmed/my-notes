# Structure and Interpretation of Computer Programs

## Applicative Order vs Normal Order (Lazy Evaluation)

- Applicative Order evaluates sub-expressions just before the procedure is applied
- Normal order passes sub-expressions as they are, without evaluation, and proceeds with the evaluation only when the corresponding formal parameter is actually to be itself evaluated.

Read [great answer here](https://cs.stackexchange.com/questions/40758/difference-between-normal-order-and-applicative-order-evaluation)

Scheme uses Applicative Order, while Haskell uses Normal Order (aka Lazy Evaluation)
