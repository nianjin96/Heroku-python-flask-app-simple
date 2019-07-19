# SIMPLE HEROKU PYTHON APP USING FLASK

Deploying python app on Heroku. Heroku is an online platform for deplying python apps.The top 3 are probably AWS, Google App Engine and Heroku.


### Prerequisites

**1.Heroku account**  
  -Go to www.heroku.com to create a free account.  
**2.Git**    
  -Go to https://git-scm.com/download/win to download Git for Windows.  
**3.Heroku Command Line Interface (CLI)**    
  -Go https://devcenter.heroku.com/articles/getting-started-with-python#set-up to download CLI.  
**4.Python**  


## Getting Started

These instructions will get you a copy of the project up and running on your local machine for development and testing purposes. See deployment for notes on how to deploy the project on a live system.

**1.Navigate to your Documents folder in terminal(I am using Cmder), and run:**    
```
Documents> mkdir heroku_first_flask_app
Documents> cd heroku_first_flask_app
Documents/heroku_first_flask_app> vim app.py
```
Put the following content into your app.py file:
```
from flask import Flask
app = Flask(__name__)

@app.route('/')
def hello_world():
    return 'Hello World!'

if __name__ == '__main__':
    app.run()
```
To run the app locally, use the following command:
```
Documents/heroku_first_flask_app> python app.py
* Running on http://127.0.0.1:5000/ (Press CTRL+C to quit)
```
You can then navigate to http://127.0.0.1:5000/ in your browser to see the 'Hello World' output.  

You have now verified that your basic Flask app is running locally, and are ready to deploy to Heroku.  

**2. Setting up your Virtual Environment**    
Next, you need to set up a virtual environment in your project folder. This virtual environment will be used to tell Heroku which Python packages your Flask app needs to use. For example, if your Flask app uses numpy to do some numerical processing on the backend, you will need to include numpy in your virtual environment. Navigate to the root of your project folder and run:  
```
Documents/heroku_first_flask_app> virtualenv venv
Documents/heroku_first_flask_app> cd venv
```
To activate your virtual environment, run:  
```
Documents/heroku_first_flask_app>.\Scripts\activate
```
(venv) should now appear at the beginning of your path in Powershell(terminal). You can deactivate the virtual environment in Powershell at any time by running .\Scripts\deactivate.  

Now that your virtual environment is activated, you are in a position to install the Python packages that Heroku requires for Flask. The packages required are as follows:  
-Flask  
-gunicorn  
-Jinja2  

You can install all of these packages with:
```
Documents/heroku_first_flask_app> pip install flask gunicorn jinja2  
```

**3. Create a Procfile (Process File)**  
Procfile is to let Heroku knows how to run this app on its platform.Each Flask app deployed to Heroku requires a Procfile. Navigate to the root of the project and run:  
```
Documents/heroku_first_flask_app> vim Procfile
```
(note: There is no postfix for Procfile,eg.Procfile.txt)  
Put the following content into your Procfile file:  
```
web: gunicorn app:app
```  

**4. Create a Requirements File**   
Heroku recognises the packages used in your Flask app by looking inside the requirements.txt file. You create your requirements file using pip and virtualenv. Running pip freeze requirements will display the packages that you have installed in your virtual environment.  
```
Documents/heroku_first_flask_app> pip freeze requirements
Flask==0.10.1
gunicorn==19.4.5
itsdangerous==0.2
Jinja2==2.8
MarkupSafe==0.23
Werkzeug==0.11.3
```

You will note that additional packages (itsdangerous, MarkupSafe and Werkzeug) have been installed. This is because running pip install flask automatically installs itsdangerous andWerkzeug, and running pip install jinja2 automatically installs MarkupSafe.  

To save these requirements to your requirements.txt file, run pip freeze > requirements.txt.  

```
Documents/heroku_first_flask_app> pip freeze > requirements.txt
```

Key Step: in Windows, your requirements.txt file will be in Unicode format by default. What the official Heroku documentation doesn't mention is that the requirements.txt file must be saved in ANSI format for the Flask app to be deployed to successfully. Open your requirements.txt file in an editor (eg Notepad), click 'Save As' and change the encoding to ANSI.  

**5. Save your Flask app using Git**  
First, create a .gitignore file:  
```
Documents/heroku_first_flask_app> vim .gitignore  
```
Open the .gitignore file and write venv. This will make git ignore the venv directory - we can do this because Heroku doesn't need to see the venv directory - it can get all the information it needs about Python packages from the requirements.txt file. Next, initialise your git repository and make a first commit:  
```
Documents/first_flask_app> git init
Documents/first_flask_app> git add --all
Documents/first_flask_app> git commit -m "first commit with app ready for deployment"
```

**6. Deploy your app to Heroku**  
```
Documents/heroku_first_flask_app> heroku login
```
Enter your Heroku email address, username and password in Powershell if prompted. Then run  
```
Documents/heroku_first_flask_app> heroku create
Creating polar-tor-1665... done, stack is cedar-14
https://polar-tor-1665.herokuapp.com/ | https://git.heroku.com/polar-tor-1665.git
Git remote heroku added
heroku-cli: Updating... done.
```

This creates a Heroku app at https://polar-tor-1665.herokuapp.com/ and adds a Git remote called heroku so that when you push your repository to the heroku remote, it will update the app URL. I can view the heroku remote in the command line with:  
```
Documents/heroku_first_flask_app> git remote -v
heroku  https://git.heroku.com/polar-tor-1665.git (fetch)
heroku  https://git.heroku.com/polar-tor-1665.git (push)
```
To deploy the app to Heroku, run(need some time to respond depence on your internet speed):    
```
Documents/heroku_first_flask_app> git push heroku master
Counting objects: 1523, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (1477/1477), done.
Writing objects: 100% (1523/1523), 5.79 MiB | 67.00 KiB/s, done.
Total 1523 (delta 241), reused 0 (delta 0)
remote: Compressing source files... done.
remote: Building source:
remote:
remote: -----> Python app detected
remote: -----> Installing runtime (python-2.7.11)
remote: -----> Installing dependencies with pip
remote:        Collecting Flask==0.10.1 (from -r requirements.txt (line 1))
remote:        Downloading Flask-0.10.1.tar.gz (544kB)
remote:        Collecting gunicorn==19.4.5 (from -r requirements.txt (line 2))
remote:        Downloading gunicorn-19.4.5-py2.py3-none-any.whl (112kB)
remote:        Collecting itsdangerous==0.24 (from -r requirements.txt (line 3))
remote:        Downloading itsdangerous-0.24.tar.gz (46kB)
remote:        Collecting Jinja2==2.8 (from -r requirements.txt (line 4))
remote:        Downloading Jinja2-2.8-py2.py3-none-any.whl (263kB)
remote:        Collecting MarkupSafe==0.23 (from -r requirements.txt (line 5))
remote:        Downloading MarkupSafe-0.23.tar.gz
remote:        Collecting Werkzeug==0.11.3 (from -r requirements.txt (line 6))
remote:        Downloading Werkzeug-0.11.3-py2.py3-none-any.whl (305kB)
remote:        Installing collected packages: Werkzeug, MarkupSafe, Jinja2, itsdangerous, Flask, gunicorn
remote:        Running setup.py install for MarkupSafe
remote:        Running setup.py install for itsdangerous
remote:        Running setup.py install for Flask
remote:        Successfully installed Flask-0.10.1 Jinja2-2.8 MarkupSafe-0.23 Werkzeug-0.11.3 gunicorn-19.4.5 itsdangero
us-0.24
remote:
remote: -----> Discovering process types
remote:        Procfile declares types -> web
remote:
remote: -----> Compressing... done, 42.8MB
remote: -----> Launching...
remote:        Released v3
remote:        https://polar-tor-1665.herokuapp.com/ deployed to Heroku
remote:
remote: Verifying deploy... done.
To https://git.heroku.com/polar-tor-1665.git
 * [new branch]      master -> master
 ```
 
 Then , you can view the result by :
```
Documents/heroku_first_flask_app> heroku open
```
you will see 'Hello World!'








## Acknowledgments

* http://www.gtlambert.com/blog/deploy-flask-app-to-heroku
