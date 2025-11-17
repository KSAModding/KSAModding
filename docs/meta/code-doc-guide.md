# Code Documentation Guidelines

This page lays out some guidlines for creating a code documentation page.
You can see an example here:
[bool2 example](/KSAModding/advanced-internals/code-docs/Brutal/Numerics/bool2/bool2/)

Make sure to always follow the [general style guide](/KSAModding/meta/style-guide/) on top of these guidlines.



## General guidline

- Pages only cover one function or datatype at a time

## Notes on incomplete documentation

Writing documentation for a full class can take A LOT of time. We do not expect you to do this all in one pull request (Incomplete documentation is better than no documentation). If you have figured out how a class works and don't feel like adding all methods just add the warning template after the short description. Same for methods, if you have only done one of the function prototypes just add the warning template and you or someone else can complete it later.

**Template:**
```
!!! warning "Documentation Incomplete"
    This documentation page is not finished yet. Some sections or method pages may still be missing.
```


## Page Location

In the docs we differentiate between 2 doc types.

1. Overarching Classes and Structs
2. Methods (Inside Classes or Structs) and Standalone Functions

### Classes and Structs (Objects)

As Objects will almost always have methods we will place the Object data in an Overview page 
Every page should be placed in a folder structure that matches its namespace and Object name.

#### bool2 Example

The page for the struct **`Brutal.Numerics.bool2`** should be located under **Code Docs** like this:

```
Code Docs
└── Brutal
    └── Numerics
        └── bool2
            └── Overview
```

### Methods

Classes can have multiple methods so each method will be contained in its own page. If you have multiple methods with the same name they are contained in the same page. This is done to make searching easier.

#### bool2.CopyTo(bool[] array)
```
Code Docs
└── Brutal
    └── Numerics
        └── bool2
            └── Overview
            └── CopyTo
```

## Structure

### Page title
The title of the page should always include the full namespace type and class if applicable.

Parameters should net yet be added to the title

#### bool2 Example
**`struct Brutal.Numerics.bool2`**
#### bool2.CopyTo(bool[] array) Example
**`void Brutal.Numerics.bool2.CopyTo`**

### Last tested version
To keep everyone on the same page, always include a version tag above the page title like this.
```markdown
<sub><sup>Tested with: **KSA Version 2025.11.4.2791**</sup></sub>
# Title
``` 

### Namespace and Assembly
After the title, add a box in the following style containing the namesapce and dll assembly.

---

Example for bool2:
```markdown
> **Namespace:** `Brutal.Numerics`  
> **Assembly:** `Brutal.Core.Numerics.dll`
``` 

### Short description
After the Namespace and Assembly, provide a short description for that specific code element.

### Code example (optional)
If you want to help users get up and running quickly, include a small example of how to use the specific function or class.

### Function prototype (Method)
After the description, add a section that lists all function protoypes for a function. In case of class or struct list the constructors instead. For a detailed example go to the [bool2 example](/KSAModding/advanced-internals/code-docs/Brutal/Numerics/bool2/bool2/) or the [CopyTo example](/KSAModding/advanced-internals/code-docs/Brutal/Numerics/bool2/CopyTo/).

### Members (classes and structs)
When writing documentation for classes or structs, if it has public members add a section for those as well. For a detailed example go to the [bool2 example](/KSAModding/advanced-internals/code-docs/Brutal/Numerics/bool2/bool2/).

### Operators
When writing documentation for classes or structs, if it has public operators add a section for those as well. For a detailed example go to the [bool2 example](/KSAModding/advanced-internals/code-docs/Brutal/Numerics/bool2/bool2/).