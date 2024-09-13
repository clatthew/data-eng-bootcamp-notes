### Persisting `count` variable
When a nested function is invoked, the entire outside function won't necessarily be invoked.
```python
def make_counter():
	count = 0
	
	def counter():
	nonlocal count
	count += 1
	return count
	
	return counter

my_counter = make_counter()

print(my_counter()) # 1
print(my_counter()) # 2
print(my_counter()) # 3
print(my_counter()) # 4

print(make_counter()()) # 1
print(make_counter()()) # 1
print(make_counter()()) # 1
print(make_counter()()) # 1
```
When the function `make_counter` is called directly, it is run each time in entirety returns the function `counter`. Then it calls `counter`, which is invoked with the second argument of `make_counter()()`.

On the other hand, when we call `my_counter`, python already knows that `make_counter()` returns `counter` and doesn't fully invoke the outside function. Therefore, `count` is not re-initialised; the previous value is modified. The `count` variable is packaged with the function `count()`, even though `make_counter` was garbage collected. This is called *closure*.

### Factory functions
Use a function which returns its nested functions to build functions which do more specific jobs.
```python
def power_factory(base):

	def power(x):
		return base ** x
	
	return power

pow2 = power_factory(2)
pow3 = power_factory(3)

print(pow2(4))
print(pow3(4))
```
In this case, in `pow2` and `pow3`, `base` is packaged in the *closure* of `power(x)`  meaning that it can continue to be used.