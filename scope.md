# Scope
`locals()` prints the current namespace as a dictionary.
also available is `globals()`. This contains the namespace available to everything in the file. Called within an function, `locals()`$\not\subset$`globals()`.

### Name resolution
Name resolution happens in the following order:
> Local scope ➟ Enclosing scope ➟ Global scope ➟ Builtin scope

All variables which are in the enclosing scope can be **read**, but **only** variables in the local scope can be **reassigned**. See an example of this [here](closures-and-decorators.md#Decorators-with-arguments), where func is used as-is, but count must be brought into the local scope using the `nonlocal` keyword in order to be reassigned.