---
id: shapes
title: Shapes
desc: ''
updated: 1680816676294
created: 1680815810826
---
Oftentimes I will refer to the "shape" of a function or data. The idea is that the shape of these things is how you determine compatibility. I think of this as a less-than-formal specification. 

## Function Shapes
Function shapes are based on the inputs and outputs. It's basically the function signature. 

Common function shapes:
> Please note `()` being used to represent 'void'

- Void: `() -> ()`
- Unary: `T -> R`
- Binary: `(T, U) -> R`
- Ternary: `(T, U, V) -> R`
- Supplier: `() -> R`
- Consumer: `T -> ()`
- Predicate: `T -> bool`
- Binary Prediate: `(T, U) -> bool`

## Data Shapes

The shape of a dataset refers to the expected fields, expected nested objects, etc. It's an informal way of saying the format. I usually use this terminology when consuming raw REST APIs, and not using an SDK to provide schema validation or object mapping. 