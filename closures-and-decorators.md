# Closures and decorators
## Persisting `count` variable
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

## Factory functions
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

There is nothing in the global scope which may be beneficial.

## Decorators
We can modify the behaviour of a function without altering its source code. A decorator
- Takes a function as its argument
- Returns another function
- Rebinds the name of the original function to the new function.
```python {11,12,13}
def subtract(a, b):
	return a - b
	
def swap(func):
	def wrap_func(a, b):
		return func(b, a)
return wrap_func

swapped_subtract = swap(subtract)

print(subtract(5, 3)) # 2
print(swapped_subtract(5, 3)) # -2
print(swap(subtract)(5, 10)) # 5

subtract = swap(subtract)

print(subtract(5, 3)) # -2
```
In the print statements on lines 11-13, `(a, b)` are the arguments of `wrap_func()`, **not** `subtract()`.
### @-syntax
We can achieve the same effect using `@`:
```python
def swap(func):
	def wrap_func(a, b):
		return func(b, a)
return wrap_func

@swap
def add(a, b):
	return a + b

print(add('3', '4')) # '43'
```

The two tests here are equivalent:
```python
import types
def tim_destroy(func):
	def inner_func(*args):
		return f'{func(*args)} meow!!'
	return inner_func

def test_decorated_func_takes_args():
	def sample_func(a, b, c=3):
		return a + b - c
	destroyed = tim_destroy(sample_func)
	assert destroyed(5, 6, 7) == "4 meow!!"

def test_decorated_func_takes_args():
	@tim_destroy
	def sample_func(a, b, c=3):
		return a + b - c
	assert sample_func(5, 6, 7) == "4 meow!!"
```

### Decorators with arguments
The decorator `before()` will only allow the passed function `func` to be run `n - 1` times.
```python
def before(n):
    def custom_before(func):
        count = 0
        def counter():
            nonlocal count
            if count < n - 1:
                count += 1
                return func()
        return counter
    
    return custom_before
```
The following test passes:
```python
def test_calls_3_times_then_stops():
    @before(4)
    def sample_function():
        return "hello"
    assert sample_function()
    assert sample_function()
    assert sample_function()
    assert not sample_function()
```
In order to pass an argument to a decorator, the inner function needs to be wrapped in a second function. The innermost function is our usual wrapped function, the middle function takes the (implicit) `func` argument, and outermost function, which is the new part, takes the decorator's argument.