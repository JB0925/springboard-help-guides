Sure. Just looked. I am pretty sure the issue here is that what you are getting back from SQLAlchemy is not actually a list of tuples, but is actually a list of SQLAlchemy row objects. It prints out and looks like a list of tuples but, functionally, it does not work the same way.
Under the line where depts is defined, do this:
```
choices = [(dept.dept_code, dept.dept_name) for dept in depts]
form.dept_code.choices = choices
```
