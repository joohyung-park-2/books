## Getting started with fp-ts: Eq
- In fp-ts type classes -> TS interfaces
```typescript
// Eq type class
interface Eq<A> {
  readonly equals: (x: A, y: A) => boolean
}
// instantiation
const eqNumber: Eq<number> = {
  equals: (x, y) => x === y
}
```
- instances must satisfy the 3 laws
  - reflexivity
    - equals(x, x) === true, for all x in A
  - symmetry: 
    - equals(x, y) === equals(y, x), for all x, y in A
  - transitivity
    - equals(x, y) === true, equals(y, z) == true
	- equals(x, z) === true, for all x, y, z in A
```typescript
functino elem<A>(E: Eq<A>): (a: A, as: Array<A>) => boolean {
  return (a, as) => as.som(item => E.equals(item, a))
}
```
- getStructEq combinator
```typescript
import { getEq, getStructEq, contramap } from 'fp-ts/lib/Eq'

type Point = {
  x: number
  y: number
}

// const eqPoint: Eq<Point> = {
//   equals: (p1, p2) => p1 === p2 || (p1.x === p2.x && p1.y === p2.y)
// }

const eqPoint: Eq<Point> = getStructEq({
  x: eqNumber,
  y: eqNumber
})

const eqArrayOfPoints: Eq<Array<Point>> = getEq(eqPoint)

type User = {
  userId: number
  name: string
}

const eqUser = contramap((user: User) => user.userId)(eqNumber)
```
