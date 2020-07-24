# Flask User Log-in Project 

## Acceptance criteria
- [ ] Redirected to homepage/index html if successful or 2nd or 3rd attempt to login. If still unsuccessful redirected to error page 404/customised page with message of your choice
- [x] Minimum 2 buttons on the login page- submit & cancel
- [x] 2 text boxes to take information in 

## Packages installed
```python
from flask import Flask, render_template
from flask_bootstrap import Bootstrap
# used for online forms
from flask_wtf import FlaskForm
from wtforms import StringField, PasswordField, BooleanField
# makes form validation easier
from wtforms.validators import InputRequired, Email, Length
```

## App.py
```python
# initialising the app
app = Flask(__name__)
app.config['SECRET_KEY'] = 'mysecretkey!'
Bootstrap(app)

# creating classes for the forms
# class for log in form
class LoginForm(FlaskForm):
    username = StringField("username", validators=[InputRequired(), Length(min=4, max=15)])
    password = PasswordField("password", validators=[InputRequired(), Length(min=8, max=80)])
    remember = BooleanField("remember me")


class RegisterForm(FlaskForm):
    email = StringField("email", validators=[InputRequired(), Email(message="Invalid Email!"), Length(max=50)])
    username = StringField("username", validators=[InputRequired(), Length(min=4, max=15)])
    password = PasswordField("password", validators=[InputRequired(), Length(min=8, max=80)])


@app.route('/')
def index():
    return render_template('index.html')


@app.route('/login', methods=['GET', 'POST'])
def login():
    # instantiating a form
    form = LoginForm()

    # checking if the form has been submitted
    if form.validate_on_submit():
        return '<h1>' + form.username.data + ' ' + form.password.data + '</h1>'
    return render_template('login.html', form=form)


@app.route('/signup', methods=['GET', 'POST'])
def signup():
    # initialising form
    form = RegisterForm()
    # checking if the form has been submitted
    if form.validate_on_submit():
        csvfile = open('users.csv')
        return '<h1> New user has been created </h>'
        # new_user = User(username=form.username.data, email=form.email.data, password=form.password.data)
        # db.session.add(new_user)
        # db.session.commit()
        # return '<h1> New user has been created </h>'
    return render_template('signup.html', form=form)


@app.route('/dashboard')
def dashboard():
    return render_template('dashboard.html')


if __name__ == '__main__':
    app.run(debug=True)

```


