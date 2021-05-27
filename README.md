# Deploying Django to Heroku!

<img src="https://i.pinimg.com/originals/3a/42/37/3a4237877cbadc9213e5007118ded912.jpg" width="150px">
<img src="https://media.tenor.com/images/18b767b668c6cf5bbb1b7d2c062c8060/tenor.gif" width="150px">
<img src="https://fiverr-res.cloudinary.com/images/t_main1,q_auto,f_auto/gigs/106095482/original/6a2a7fc989e1b0add530c7bfbbc5c22eed2cf379/do-any-python-or-django-task.png" width="150px">

## Set up Heroku

Sign up for [Heroku](https://id.heroku.com/login)

Install the [heroku CLI](https://devcenter.heroku.com/articles/heroku-cli)

Open terminal in your project and login to your account. 

``` bash 
$ heroku login
Enter your Heroku credentials.
Email: {your heroku email}
Password (typing will be hidden):
Authentication successful.

```

## Preparing The Application

When your project is ready for deployment here are the key steps you have to take. 

- Add a Procfile
- Update requirements.txt
- Install Gunicorn and Heroku DJango
- Add a runtime.txt to specify the correct Python version in project root
- Configure whitenoise to serve static files

### The Procfile 
___

Create a file named `Procfile` (without a file extension) in the root of your project directory with the following info.

```
web: gunicorn {name of your project folder}.wsgi
```

Replace the {} and the contents with the name of your project folder, for example
```
web: gunicorn catcollector_project.wsgi
```

### The Runtime.txt
___

In terminal run..

```
python3 -V
``` 
this will give you your python version you are using for your project. 

Create a file called `runtime.txt` in the root project directory with the following info about your python project.

```
python-{your version number}

```

[Supported Python Runtimes](https://devcenter.heroku.com/articles/python-support#supported-runtimes)

### Install Gunicorn and Django-On-Heroku
___

In your project folder, with the environment activated, run the following..

```
$ pip3 install django-on-heroku

```
This will install the helper addons for heroku. Now we must add it into our project. 
Add to your settings.py 

```python
# after "from pathlib import Path"
import django_on_heroku

# at the bottom of the file
django_on_heroku.settings(locals())
```
Now to install Gunicorn and Whitenoise! 

```
$ pip3 install gunicorn

```

Install Whitenoise 

``` bash
$ pip3 install whitenoise
```

### Update requirements.txt
___

Update your existing Pipfile, or create a new Pipfile with the following command...

```
pip3 freeze > requirements.txt

```

### Git Add & Commit Changes
___

Add all Heroku config changes
```
git add -A

```

Commit changes
```
git commit -m "Adds heroku deployment config"

```

## Time to Deploy! 

Inside your project folder run..

```
heroku create [project name] --buildpack heroku/python

```
This will take some time as heroku sets up your heroku remote git.

Once that is complete its time to add a Database to our heroku application.

``` bash
heroku addons:create heroku-postgresql:hobby-dev

```

Now you can git add and git commit.

Once that is complete you can now push to Heroku! 

``` bash
$ git push heroku master

```

Once that is completed we need to migrate our database just as we would on our own devices.

OPEN THE HEROKU CONSOLE

```
$ heroku run bash

```

INSIDE THE HEROKU CONSOLE RUN

```
$ python3 manage.py makemigrations

```

```
$ python3 manage.py migrate

```

EXIT THE HEROKU CONSOLE

```
$ exit

```

And viola! Your Django application is officially online! You can run the following command to view it. 


``` bash
$ heroku open

```

Once you have tested your application and everything is lookging great it's time to turn off debug mode! 

in settings.py 

```python
# SECURITY WARNING: don't run with debug turned on in production!
DEBUG = False # change this line to false
```

now just add, commit and push up to heroku. 

Happy Coding! 

![coder](https://media.giphy.com/media/ZVik7pBtu9dNS/giphy.gif)
