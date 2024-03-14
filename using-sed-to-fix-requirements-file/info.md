Thanks. Yeah, that is the newest version of python and version 1.1.1 of Flask was made when python was at 3.7 or 3.8. You best bet is to run pip uninstall inside of your venv, then remove all of the ==1.1.1 from your requirements.txt file.
You can do that quickly like this:
```
sed 's/==[0-9.]*//g' requirements.txt > tmp.txt

cat tmp.txt - make sure tmp.txt looks correct and does not have the ==

mv tmp.txt requirements.txt
```
Now your requirements.txt file will just have the names of the dependencies. This will make it so that the latest versions are installed.
Finally, inside of your venv, run pip install -r requirements.txt
Note: Not just ==1.1.1, but all of the == in requirements.txt.
