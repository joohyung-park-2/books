## Ch6. Advanced Types
- supports powerful type-level programming features
  - expressive, easy to declaring type constraints & relationships inferred
  - because of JS dynamicity(prototypes, dynamically bounded this, function overloads, always changing objects)
### Relationshipts Between types
#### Subtypes and Supertypes
- Rule of thumb
  - If type B is a subtype of type A(B <: A) or If type A is a supertype of type B(A >:B)
  - then you can safely use a B anywhere an A is required
#### Variance
- Subtyping rules for complex types
  - harder to reason about, not clear-cut
  - a big point of disagreement among programming languages
##### Shape and array variance
- TS shapes(object/class) are covariant in their property types
  - each of its properties must be <: its corresponding property of the expected type
- 4 sorts of variance
  - invariance: A <: && >: B
  - covariance: A <: B
  - contravariance: A >: B
  - bivariance: A <: || >: B
- In TS, every complex type is covariant in tis members
  - with one exception: **function parameter types**, which are **contravariant**
##### Function variance
- Func A is a sub type of func B
  - if A has the same/lower arity than B &&
  - A's this is unspecified || A's this >: B's this
  - A's param >: coresponding B's param
  - A's return <: B's return
``` typescript
class Animal {}
class Bird extends Animal {
  chirp() {}
}
class Crow extends Bird {
  caw() {}
}

function clone(f: (b: Bird) => Bird): void {
  let parent = new Bird
  let babyBird = f(parent)
  babyBird.chirp()
}

function birdToAnimal(d: Bird): Animal {
  // ...
}
clone(birdToAnimal) // Error

function crowToBird(c: Crow): Bird {
  // ...
}
clone(crowToBird) // Errorf
```
#### Assignability
- A is assignable to B
  - Non-enum types
    - A <: B
    - A is any: exception of the 1st rule, for a convenience for inteoperating with JS code
  - Enum types
    - A is a member of enum B
	- B has at least on number as a member, A is a number ??? one of unsafety in TS
#### Type Widening(TBD)
- key to understanding TS type inference
#### Refinement(TBD)
### Totality
- exhaustiveness checking to make sure you've covered all your cases
- no matter what kind of control structure you use - switch, if, throw, ...
### Advanced Object Types
#### Type Operators for Object Types
##### The keying-in/keyof(TBD) operator
``` typescript
type APIResponse = {
  user: {
    userId: string
	friendList: {
	  count: number
	  friends: {
        firstName: string
        lastName: string
	  }[]
	}[]
  }
}
function renderFriendList(friendList: unknown) {
  // ...
}
function renderFriendList(friendList: APIResponse['user']['friendList']) {
  // ...
}
type FriendListKeys = keyof APIResponse['user']['friendList'] // 'count' | 'friends'
```
- keys allowed to be a regular string, number, or symbol only for a regular index
#### The Record Type
- a way to describe an object as a map from sth to sth
- keys constrained to subtypes of string and number
```typescript
let nextDay: Record<Weekday, Day> = {
  // ...
}
```
#### Mapped Types
```typescript
let nextDay: {[K in WeekDay]: Day} = {
  // ...
}
```
