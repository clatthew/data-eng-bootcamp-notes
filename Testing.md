# Testing
### Using pytest
Tests take place within functions in the *tests* file. These functions have long, descriptive names, starting with *test*, written in `test_snake_case`.

Use `assert` to set the condition for a test passing. This comes at the end of the function instead of return. Syntax: `assert [boolean], [message if test fails]`.

Change the path variable using ``export PYTHONPATH=$(pwd)``
Run a test using `pytest`

Useful tags:
- `-vvvrP`. This includes prints in the output and specifies which tests have passed.
- `-k` targets a single test
Pytest always looks for a folder called *tests* and runs all of the .py files in this folder.
### The testing cycle
1. Write test for the simplest behaviour
2. Write the minimum code to pass the test
3. Test the code
4. Refactor
5. Test again
6. Write a test for the next simplest behaviour
7. Test
8. etc.

If a test passes the first time that it is run, it is good practice to force it to fail in order to verify that it is working as intended.

### Pytest-testdox
uses @decorators
```python
@pytest.mark.it('description')
```
replaces the test name in the output log when positioned above a function definition.

### Template test file
Arrange ➟ act ➟ assert.
```python
from src.file import function
from pytest import mark


@mark.it('')
def test_():
	test_data =
	expected =
	result = function(test_data)
	assert result == expected
```

### Troubleshooting
Have you...
- set your `PYTHONPATH` variable
- set function name different to the file which contains it
- put `test_` at the front of all test function names

### Testing functions which don't return anything
Some functions affect change but don't return anything. Therefore we can't use their output to test for correct operation.
#### Approach 1:
For a function which takes a function as an argument, check that the argument function has been called the expected number of times.

In the following example, a the function `repeater` takes arguments `func`, `call_count` and `arg_to_pass` but *doesn't return anything*.
```python
def test_calls_the_given_function():
	calls = 0
	
	def test_func():
		nonlocal calls
		calls += 1

	test_call_count = 2
	test_arg = "hello"
	repeated(test_func, test_call_count, test_arg)
	assert calls == 2
```

#### Approach 2:
Use mock functions using the `unittest.mock` built-in functions.
```python
from unittest.mock import Mock
matthew = Mock()
matthew.called # False
matthew()
matthew.called # True
matthew.call_count
matthew.reset_mock()
matthew.call_args.args
matthew.call_args_list
```