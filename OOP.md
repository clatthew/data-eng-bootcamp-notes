# Object-oriented programming
## Classes and their methods and attributes
### A basic class
```python
class Person:
	def __init__(self, name):
		self.name = name
		self.apple_count = 0

	def greet(self):
		return 'Hi there'

	def eat(self, fruit):
		if fruit == 'apple':
			if self.apple_count < 3:
				print('yum yum')
				count += 1
			else:
				print("I'm too full")

	def digest(self):
		self.apple_count -= 1

test_person = Person('Matt')
print(test_person.name) # 'Matt'
print(test_person.greet) # 'Hi there'
print(test_person.eat('apple')) # 'yum yum'
print(test_person.eat('apple')) # 'yum yum'
print(test_person.eat('apple')) # 'yum yum'
print(test_person.eat('apple')) # "I'm too full"
print(test_person.digest())
print(test_person.eat('apple')) # 'yum yum'
```
### Tests in classes
Pytest tests can be separated into classes in order to group them, for example, by method. This looks really nice with pytest-testdox.
```python
class TestEat:
	def test_eats_apple(self):
		test_person = Person('Matt')
		test_person.eat('apple')
		assert test_person.apple_count == 1
	def test_max_3_apples(self):
		# etc...
		
```
### Class attributes
Class attributes belong to the class, not to the object. They are declared outside of methods.
```python
class TVShow:
	streaming_service = 'NowTV'
	def __init__(self, name, no_episodes):
		self.name = name
		self.no_episodes = no_episodes

veep = TVShow('Veep', 62)
the_thick_of_it = TVShow('The Thick of It', 35)
veep.streaming_service # NowTV
the_thick_of_it.streaming_service # NowTV
the_thick_of_it.streaming_service = 'BBC iPlayer'
the_thick_of_it.streaming_service # BBC iPlayer
```
### Class methods
Class methods take their class as argument, opposed to *instance methods*, which take their object. Marked using the `@classmethod` decorator.
```python
class TVShow:
	streaming_service = 'NowTV'
	def __init__(self, name, no_episodes):
		self.name = name
		self.no_episodes = no_episodes

	@classmethod
	def copy(cls, instance_to_copy):
		new_instance = cls(
			instance_to_copy.name,
			instance_to_copy.no_episodes)
		return new_instance

veep = TVShow('Veep', 62)
veep_copy = TVShow.copy(veep)
print(veep_copy.__dict__) # looks the same as veep.__dict__
```
### Static methods
Functions which belong to the class but do not take the `self`. Marked using the `@staticmethod` decorator.
```python
class TVShow:
	streaming_service = 'NowTV'
	def __init__(self, name, no_episodes, no_specials):
		self.name = name
		self.no_episodes = no_episodes
		self.specials = specials
		self.total_episodes = self.no_episodes + self.specials
		
	@staticmethod
	def add(a, b):
		return a + b

veep = TVShow('Veep', 62, 5)
veep.total_episodes # 67
```
## Private attributes and methods
### Name mangling
Attributes starting with double underscore are private. This means they cannot (easily) be accessed from outside the object. Calling `object_name.__private_attribute` will return an error.
Private attributes can be accessed anyway using `object_name._ClassName__private_attribute`.
The same applies to private methods.
### Getters and Setters
We can make private attributes viewable without risk by returning a `deepcopy`. This is an example of a *getter* method. Accordingly, we also use *setter* methods to set/update private attributes. Setter and getter methods are identified using `@property` and `@attribute_name.setter` decorators (lines 6 and 10 below).
> **Note:** getters and setters do not need parameters when invoked. **They look like attributes**.

### Example
Before implementing getters and setters:
```python
class Example:
	def __init__(self):
		self.__storage = [1, 2, 4]

test_class = Example()
test_class.__storage.append(5) # error
print(test_class.__storage) # error
```
After implementing getters and setters:
```python {6,10}
from copy import deepcopy
class Example:
	def __init__(self):
		self.__storage = [1, 2, 4]
		
	@property
	def storage(self):
		return deepcopy(self.__storage)
		
	@storage.setter
	def storage(self, new_val):
		if 5 in new_val:
			self.__storage = new_val

test_class = Example()
print(test_class.__storage) # error: "did you mean test_class.storage?"
print(test_class.storage) # [1, 2, 4]
test_class.__storage = ([5, 5, 5]) # does nothing
test_class.storage = ([4, 4, 4]) # does nothing
test_class.storage = ([5, 5, 5])
print(test_class.storage) # [5, 5, 5]
```