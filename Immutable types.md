These types are stored to a new location in memory when "modified" (eg. appended).
When declared, immutable types with the same value are assigned the same position in memory.
Immutable types include `int`, `float`, `bool`, `string`, `Unicode` and `tuple`.

You can use `deepcopy` from the `copy` library which will copy the outer level (eg. list, dict) as well as all contained and nested objects.