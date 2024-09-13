`locals()` prints the current namespace as a dictionary.
also available is `globals()`. This contains the namespace available to everything in the file. Called within an function, `locals()`$\not\subset$`globals()`.

### Name resolution
Happens in the following order:
Local scope ➟ Enclosing scope ➟ Global scope ➟ Builtin scope
