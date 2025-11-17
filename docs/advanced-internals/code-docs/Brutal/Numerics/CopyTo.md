<sub><sup>Tested with: **KSA Version 2025.11.4.2791**</sup></sub>
# struct Brutal.Numerics.bool2.CopyTo
> **Namespace:** `Brutal.Numerics`  
> **Assembly:** `Brutal.Core.Numerics.dll`

Copies the components of the instance into a provided array or span.  
Depending on the overload, copying begins at index `0`, a specified index, or into a `Span<bool>`.

## Function Prototypes

### `void CopyTo(bool[] array)`

Copies the components into an array starting at index `0`.  
The target array must have a length of **at least 2**.

**Parameters:**  

  * `array` — The destination array that will receive the components.

### `void CopyTo(bool[] array, int index)`

Copies the components into an array starting at the specified `index`.

**Parameters:**  

  * `array` — The destination array that will receive the components.  
  * `index` — The starting index in the array where copying begins.s

### `void CopyTo(Span<bool> destination)`

Copies the components into a `Span<bool>`.  
The destination span must have a length of **at least 2**.

**Parameters:**  

  * `destination` — The span that will receive the components.