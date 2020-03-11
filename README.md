# OIS Onboarding
Wiki with info for incoming committees.

# Recommendation
I recommend learning about [Git](https://try.github.io/) and [Django](https://www.djangoproject.com/start/).
We are using heroku to host our website for free, and git for deployment and version control.
Django is a python based webapp framework, which is used to create the entire website.

## Requirements
First you need to have:
* A [Github](https://github.com/) account 
* Install [Python 3](https://www.python.org/downloads/) and pip3
* Install [Heroku CLI](https://devcenter.heroku.com/articles/heroku-cli)

## Steps

### Install [VirtualEnv](https://docs.python.org/3/library/venv.html)
After installing python3, you need to install VirtualEnv. This is really important to keep the website deployment clean. Otherwise you local python packages will cause problems when you will try to update the website from your computer.

### Get the website source code

Open a terminal and navigate to a folder where you want to keep the source code of the OIS website. Now follow these instructions.

First login to Heroku.Use this command and follow the instructions
```
$ heroku login
```
Then clone the repository and enter inside the source directory
```
$ heroku git:clone -a oissite
$ cd oissite
```
Now you have got a directory called `oissite` which contains the source code of the OIS website.

### Create a VirtualEnv
This step is different for different operating systems. Sometimes it also depends on the OS versions. So I will suggest to check online how to do it. This is how it sould work for Ubuntu 16:
First make sure you have pip3 installed
```
$ sudo apt-get install python3-pip
```
Make sure virtualenv is installed
```
$ sudo pip3 install virtualenv 
```
**we are using `pip3` to make sure virtualenv is installed for python3. If you don't have python2 installed, it may not work. Then you have to use `pip` without that `3`**

Now crate a virtual environment called `venv` (If you change this name, make sure you add that to the .gitignore file, otherwise this entire folder will be added to the project, which may cause problems later). *Note: this variable is not a part of the project and should not be added. This is just to make your life easier*
Make sure you are inside the `oissite` directory
```
$ virtualenv venv 
```

## Start the VirtualEnv
start the virtualenv `venv`, that we just created
```
$ source venv/bin/activate
```
Now you will see `(venv)` at the beginning of each line on your terminal, which means python3 is using this virtualenv to separate the python packages for this project from your local pre-installed packages. This is really useful to make sure you are using the right version of each package, instead of using any pre-installed python packages.

### Install python packages
This is a **one Time** step. You only have to install the packages once when you do it first time or if you want to use a different computer.
**Before doing this step, make sure yout virtualenv is activated (previous step)**
Inside the `oissite` directory, there is a `requirements.txt` file which contains a list of python packages needed for this project. **This is REALLY IMPORTANT that this file is maintained properly. If you need to use a new python package, make sure you add that to this file. THIS FILE IS USED BY THE SERVER TO DETERMINE WHICH PYTHON PACKAGES ARE NEEDED FOR THIS PROJECT**

To Install the packages, execute this command:
```
$ pip3 install -r requirements.txt
```
Now all these packages are installed under your virtualenv. So these packages will not be available if the virtualenv `venv` is not activated.
If there the package installation fails for some reason, I will recommand search the error message online or check on stackoverflow. If you still don't find anything, cantact me.
A common error is like this:

```
Error: pg_config executable not found.
    
    pg_config is required to build psycopg2 from source.  Please add the directory
    containing pg_config to the $PATH or specify the full executable path with the
    option:
    
        python setup.py build_ext --pg-config /path/to/pg_config build ...
    
    or with the pg_config option in 'setup.cfg'.

```
An easy fix will be to install `libpq-dev` by doing `$ sudo apt-get install libpq-dev`. For more information about this error, [check here](https://stackoverflow.com/questions/11618898/pg-config-executable-not-found).

### Add local settings
Add this local.py file inside `/oissite/oissite/settings/`where production.py already exist. This local.py is added to the .gitignore so this file is not part of the project and **make sure is never goes to the server**.
production.py contains all the app settings for the live website, which is different from the local settings.


## Start the Web App
Finally you can start the web app server locally to see your changes before you actually make the changes live.
```
$ python3 manage.py runserver
```
**Just like `pip3`, we are using `python3` to avoid python version conflict**
This command will start a local server under port `8000`, which means your computer is working like a server for ther web app. Don't close the terminal or don't press CONTROL-C  
Now open a browser(ideally Google Chrome) and go to http://127.0.0.1:8000/
**https will not work locally, so always use http to run the project locally**
Remember, this uses a local database, which is currently empty, so you are not gonna see any blogs, events or committee archive. You can create some new local entires by going to http://127.0.0.1:8000/admin, but to login, you need the super user password. For this project, I have already created a super user, you can change the password locally (as you are running the server locally). This user and password has **nothing to do with the live website**

### Change password of the Local Super User
Stop ther server by pressing CONTROL-C and execute this command:
```
$ python3 manage.py changepassword OIS
```
`OIS` is the user name, and now you can set new password. Then start the local server again and go to http://127.0.0.1:8000/admin and login using username `OIS` and the password you selected


## Update the website
If you make any changes to the source code, you have to follow these steps to deploy the changes to the server (This will not affect the online database, but will affect the static files)
First add the changes to git
```
$ git add <file you have modified or added>
```
you can also use `$ git add .` under your root directory (which is the top level `oissite` directory) to add all the changes to git, but this it more risky as you may add some unwanted files or changes

Now commit your changes:
```
$ git commit -m "a nice commit message that describes the changes you have made. Note that these quotes are needed"
```
Now you have to push these changes to your server.
Make sure you are logged into heroku on this terminal, then execute this:
```
$ git push heroku master
```
This may take some time, but when this is done, go to ois.org.uk and you will see your changes there.
