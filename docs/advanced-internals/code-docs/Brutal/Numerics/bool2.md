<sub><sup>Tested with: **KSA Version 2025.11.4.2791**</sup></sub>
# struct Brutal.Numerics.bool2
> **Namespace:** `Brutal.Numerics`  
> **Assembly:** `Brutal.Core.Numerics.dll`

Represents a pair of booleans.


!!! warning "Documentation Incomplete"
    This documentation page is not finished yet. Some sections or method pages may still be missing.

## Example

```c#
public void OnFullyLoaded()
{
    bool2 example = new bool2(true, false);

    if(example.YX == new bool2(false, true))
    {
        Console.WriteLine("The swizzle worked!");
    }
}
```


## Constructors

### `bool2(bool x, bool y)`

Creates a `bool2` from two explicit boolean values.

**Parameters:**

  * `x` — Value for the `X` component
  * `y` — Value for the `Y` component

---

### `bool2(bool value)`

Creates a `bool2` where both components (`X` and `Y`) are set to the same boolean value.

**Parameters:**

  * `value` — The boolean assigned to both components

---

### `bool2(ReadOnlySpan<bool> span)`

Creates a `bool2` from the first two elements of a `ReadOnlySpan<bool>`.

**Parameters:**

  * `span` — A span containing at least two boolean values

---

### `static bool2 Load(in ReadOnlySpan<bool> span)`

Creates a `bool2` from a span, equivalent to calling the span constructor.

**Parameters:**

  * `span` — A read-only span containing the values

## Members

### Fields

#### `bool X`

The X component of the vector.
XML-serializable.

#### `bool Y`

The Y component of the vector.
XML-serializable.

---

### Indexer

#### `bool this[int index]`

Gets or sets a component by index.

* 0 → X
* 1 → Y

Throws on out-of-range indices.

---

### Static Properties

#### `int ComponentSize`

The size of each component in bytes. Always `1`.

#### `int Count`

The number of components in the vector. Always `2`.

#### `bool2 Zero`

A `bool2` with both components set to `false`.

#### `bool2 One`

A `bool2` with both components set to `true`.

#### `bool2 UnitX`

A vector where `X = true`, `Y = false`.

#### `bool2 UnitY`

A vector where `X = false`, `Y = true`.

---

### Swizzle Properties

#### `bool2 XY`

Gets or sets the vector as `(X, Y)`.

#### `bool2 YX`

Gets `(Y, X)` or sets by reversing assignment order.

#### `bool R`

Alias for component `X`. (Red)

#### `bool G`

Alias for component `Y`. (Green)

#### `bool2 RG`

Alias for `(R, G)` same as `(X, Y)`.

#### `bool2 GR`

Alias for `(G, R)` same as `(Y, X)`.


## Operators

### Equality Operators

#### `bool operator ==(bool2 left, bool2 right)`
#### `bool operator !=(bool2 left, bool2 right)`

### Boolean Operators

#### `bool2 operator |(bool2 a, bool2 b)` — Componentwise OR
#### `bool2 operator &(bool2 a, bool2 b)` — Componentwise AND

