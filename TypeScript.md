- [Interface](#interface)
- [Tuples and Anums](#tuples-and-anums)


# Interface
-allow to setup shape for object (only objects)

```ts
    interface Book {
        readonly isbn: number;
        title: string;
        author: string;
    }

    const deepWork: Book = {
        isbn: 56265426526,
        title: 'Lord of the rings',
        author: 'rafal',
    }
```
# Tuples and Anums

Tuple destructuring

Tuples may be destructured like arrays; the destructuring variables get the types of the corresponding tuple elements:


```ts
let tuple: [number, string, boolean] = [7, "hello", true];
let [a, b, c] = tuple; // a: number, b: string, c: boolean
```

**Enums** are one of the few features TypeScript has which is not a type-level extension of JavaScript.

Enums allow a developer to define a set of named constants. Using enums can make it easier to document intent, or create a set of distinct cases. TypeScript provides both numeric and string-based enums.
Numeric enums

We’ll first start off with numeric enums, which are probably more familiar if you’re coming from other languages. An enum can be defined using the enum keyword.

```ts
enum Direction {
  Up = 1,
  Down,
  Left,
  Right,
}
```

Above, we have a numeric enum where Up is initialized with 1. All of the following members are auto-incremented from that point on. In other words, Direction.Up has the value 1, Down has 2, Left has 3, and Right has 4.

If we wanted, we could leave off the initializers entirely:

```ts
enum Direction {
  Up,
  Down,
  Left,
  Right,
}
```

Here, Up would have the value 0, Down would have 1, etc. This auto-incrementing behavior is useful for cases where we might not care about the member values themselves, but do care that each value is distinct from other values in the same enum.

Using an enum is simple: just access any member as a property off of the enum itself, and declare types using the name of the enum:

```ts
enum UserResponse {
  No = 0,
  Yes = 1,
}
 
function respond(recipient: string, message: UserResponse): void {
  // ...
}
 
respond("Princess Caroline", UserResponse.Yes);
```