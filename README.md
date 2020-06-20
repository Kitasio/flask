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