# Regex in Python
```PYTHON
import re
string = 'string to be searched'
regex = re.compile(r'[regex expression]', re.IGNORECASE)
regex.search(string)
regex.search(string).group(2)
regex.findall()
```

Character groups are enclosed in (parentheses) and can be referenced with the `.group()` method.

Match a repeated character with `.*(.*).*\1.*`

Create an iterable object containing all instances of a match using
```compile(rf"[{char_type}]").finditer(token)]```

