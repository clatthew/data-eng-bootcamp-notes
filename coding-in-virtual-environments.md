# Coding in a virtual environment
## Creating the virtual environment
1. docs.python.org/3/library/venv.html
2. `python -m venv name`
3. The `-m` causes the *venv* module to be executed.
4. By convention, the name is set as `venv`.
5. The *venv* folder contains its own *site-packages* folder.

## Starting the virtual environment
1. `source venv/bin/activate`
2. Create a `.gitignore` file and add the *venv* folder.
3. Add all dependency names to `requirements.txt`
4. `pip install -r requirements.txt`
## Finding requirements
Run `pip freeze > requirements.txt` in terminal. `requirements.txt` should contain all necessary packages and be pushed to github.

## Closing the virtual environment
When finished developing, run `source deactivate` in the terminal.
