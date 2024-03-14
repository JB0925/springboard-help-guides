In iPython, you should be able to import text like this:
```
from sqlalchemy import text
```
Then you can run your query like this:
```
query = text(“SELECT * FROM subreddits”)
result = db.session.execute(query)
# act on or view the result 
```
