# Scope
`locals()` prints the current namespace as a dictionary.
also available is `globals()`. This contains the namespace available to everything in the file. Called within an function, `locals()`$\not\subset$`globals()`.

### Name resolution
Name resolution happens in the following order:
> Local scope ➟ Enclosing scope ➟ Global scope ➟ Builtin scope

All variables which are in the enclosing scope can be **read**, but **only** variables in the local scope can be **reassigned**. See [here](closures-and-decorators.md#^287cfe).