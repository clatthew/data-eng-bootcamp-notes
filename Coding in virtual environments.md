### Creating the virtual environment
docs.python.org/3/library/venv.html
`python -m venv name`
The `-m` causes the *venv* module to be executed.
By convention, the name is set as `venv`.
The *venv* folder contains its own *site-packages* folder.

### Starting the virtual environment
`source venv/bin/activate`
Create a `.gitignore` file and add the *venv* folder.
Add all dependency names to `requirements.txt`
`pip install -r requirements.txt`
### Finding requirements
`pip freeze > requirements.txt`
These should be pushed to github.

When finished developing, use `source deactivate`
