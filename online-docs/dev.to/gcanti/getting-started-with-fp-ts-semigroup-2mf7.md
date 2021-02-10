## Getting started with fp-ts: Semigroup
- semigroup
  - a fundamental abstraction of FP
  - captures the concept of "merging" values via concat
### General definition
- semigroup: a pair (A, *)
  - A: a non-empty set
  - *: a binary associative operation
    - *: (x: A, y: A) => A
    - (x * y) * z = x * (y * z)   x,y,z in A
  - capture the essence of parallelizable operations
### Type class definition
```typescript
interface Semigroup<A> {
  concat: (x: A, y: A) => A
}
```
