# Quickstart with flask
```
from flask import Flask
app = Flask(__name__)

@app.route('/')
@app.route('/home')
def hello_world():
    return 'Hello, World!'

if __name__ == '__main__':
    app.run(debug=True)
```
Every decorator is a link, can use multiple

---
# Templates 
Create `templates` folder where you will store all your html files.

Import `render_templates`

Create `layout.html`, here repeated code will be stored

### layout.html
```
<!DOCTYPE html>
<html>
<head>
    {% if title %}
        <title>Flask Blog - {{ title }}</title>
    {% else %}
        <title>Flask Blog</title>
    {% endif %}
</head>
<body>
    {% block content %}{% endblock %}
</body>
</html>
```
### about.html
```
{% extends "layout.html" %}
{% block content %}
    <h1>bla bla text</h1>
{% endblock content %}
```

# Static

Make a static folder for all the images, css, js files etc

Import `url_for`

example of url_for usage:
```
<link rel="stylesheet" type="text/css" href="{{ url_for('static', filename='main.css') }}"
```
---

# Forms
`pip install flask-wtf`
`pip install email-validator`

Example:
```
from flask_wtf import FlaskForm
from wtforms import StringField, PasswordField, SubmitField, BooleanField
from wtforms.validators import DataRequired, Length, Email, EqualTo

class RegistrationForm(FlaskForm):
    username = StringField('Username', validators=[DataRequired(), Length(min=2, max=10)])
    email = StringField('Email', validators=[DataRequired(), Email()])
    password = PasswordField('Password', validators=[DataRequired()])
    confirm_password = PasswordField('Password', validators=[DataRequired(), EqualTo('password')])

class LoginForm(FlaskForm):
    email = StringField('Email', validators=[DataRequired(), Email()])
    password = PasswordField('Password', validators=[DataRequired()])
    remember = BooleanField('Remember me')
    submit = SubmitField('Login')
```

# Deployment

`sudo apt install nginx`
`pipenv install gunicorn`
`sudo apt install supervisor`

nginx handles all the static files but leaves all the python work to gunicorn

1. remove nginx default file `sudo rm /etc/nginx/sites-enabled/default`
2. create your own file `sudo vim /etc/nginx/sites-enabled/your_filename`
```
server {
    listen 80;
    server_name <ip address>;
    
    location /static {
        alias /home/kit/flask_dir/static;
    }
    
    location / {
        proxy_pass http://localhost:8000;
        include /etc/nginx/proxy_params;
        proxy_redirect off;
    }
}
```
3. restart the nginx server `sudo systemctl restart nginx`
4. now gunicorn, check how many cores there is on your machine `nproc --all` to know the number of workers for gunicorn which is (2 * num_cores) + 1
5. for gunicorn to run in the background we configure supervisor `sudo vim /etc/supervisor/conf.d/file_name.conf
```
[program:your_project_name]
directory=/home/kit/project_dir
command=/home/kit/.local/share/virtualenvs/belarus-eYR9BXFo/bin/gunicorn -w 3 run:app
user=kit
autostart=true
autorestart=true
stopasgroup=true
killasgroup=true
stderr_logfile=/var/log/project_dir/project.err.log
stdout_logfile=/var/log/project_dir/project.out.log
```
6. create that log files
7. `sudo supervisorctl reload`
