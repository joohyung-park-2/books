## Getting started with fp-ts: Category
- 1st advanced abstration -> functor
  - fuctors are built upon categories
- Composition: a corner stone of fp
- Categories capture the essence of composition
### Categories
#### Part I (Definition)
- Category: a pair (Objects, Morphisms)
- Morphisms: morphisms between the objects
  - each morphism f has a source A, and a target B where both are in Objects
  - f is a morphism from A to B
#### Part II (Composition)
- composition of morphisms
  - f: A -> B, g: B -> C
  - g.f: A -> C
- associativity
  - h.(g.f) = (h.g).f
- identity
  - identity: X->X
  - f: A->X, identity.f = f
  - g: X->B, g.identity = g
#### Categories as programing languages(Typescript)
- objects: types
- morphisms: functions
- identity morphisms
  - ```typescript const identity = <A>(a: A): A => a```
- .: functions composition
#### The central problem with composition
- f: (a: A) => B, g: (c: C) => D
- B = C
  - ```typescript
  function compose<A, B, C>(g: (b: B) => C, f: (a: A) => B): (a: A) => C {
    return a => g(f(a))
  }
  ```
- B != C??? -> functors

