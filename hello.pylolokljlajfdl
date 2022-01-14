from flask import Flask, abort, redirect, url_for, render_template, send_file
from flask.json import jsonify
from joblib import dump, load
import numpy as np
from sklearn import datasets
from flask import request
import os
import pandas as pd 

knn = load('knn.pkl')

app = Flask(__name__)

@app.route("/")
def hello_world():
    print('HI')
    return "<h1>Hello, my best Friend!</h1>"

@app.route('/user/<username>')
def show_user_profile(username):
    # show the user profile for that user
    username = float(username) * float(username)
    return f'User {username}'

@app.route('/avg/<nums>')
def avg(nums):
    # show the user profile for that user
    nums = nums.split(',')
    nums = [float(x) for x in nums]
    nums = sum(nums) / len(nums)
    return f'User {nums}'

@app.route('/iris/<params>')
def iris(params):
    
    nums = params.split(',')
    nums = np.array([float(x) for x in nums]).reshape(1, -1)
    res = knn.predict(nums)
    return str(res)

@app.route('/show_image')
def show_image():
    return '<img src="/static/Irissetosa1.jpeg" alt="Iris setosa">'

@app.route('/badrequest400')
def bad_request():
    return abort(400) 

@app.route('/iris_post', methods=['POST'])
def add_message():
    try:
        content = request.get_json()
        params = content['flower']
        nums = params.split(',')
        nums = np.array([float(x) for x in nums]).reshape(1, -1)
        print('lol', nums)
        res = knn.predict(nums)
        predict = {'class': str(res[0])}
    except:
        return redirect(url_for('bad_request'))
    
    return jsonify(predict)


from flask_wtf import FlaskForm
from wtforms import StringField, FileField
from wtforms.validators import DataRequired
from flask_wtf.file import FileField, FileRequired
from werkzeug.utils import secure_filename

class MyForm(FlaskForm):
    name = StringField('name', validators=[DataRequired()])
    file = FileField()

app.config.update(dict(
    SECRET_KEY="powerful secretkey",
    WTF_CSRF_SECRET_KEY="a csrf secret key"
))

@app.route('/submit', methods=['GET', 'POST'])
def submit():
    form = MyForm()
    if form.validate_on_submit():
        print(form.name.data)
        f = form.file.data
        filename = form.name.data + '.csv'
        # f.save(os.path.join(
        #     filename
        # ))
        df = pd.read_csv(f, header=None)
        predict = knn.predict(df)
        result = pd.DataFrame(predict)
        result.to_csv(filename, index=False)
        print(predict)
        # return 'file uploaded'
        return send_file(filename,
                     mimetype='text/csv',
                     attachment_filename=filename,
                     as_attachment=True)
    return render_template('submit.html', form=form)


import os
from flask import Flask, flash, request, redirect, url_for
from werkzeug.utils import secure_filename

UPLOAD_FOLDER = ''
ALLOWED_EXTENSIONS = {'txt', 'pdf', 'png', 'jpg', 'jpeg', 'gif'}

app.config['UPLOAD_FOLDER'] = UPLOAD_FOLDER


def allowed_file(filename):
    return '.' in filename and \
           filename.rsplit('.', 1)[1].lower() in ALLOWED_EXTENSIONS

@app.route('/upload', methods=['GET', 'POST'])
def upload_file():
    if request.method == 'POST':
        # check if the post request has the file part
        if 'file' not in request.files:
            flash('No file part')
            return redirect(request.url)
        file = request.files['file']
        print(file)
        # If the user does not select a file, the browser submits an
        # empty file without a filename.
        if file.filename == '':
            print('---', file)
            flash('No selected file')
            return redirect(request.url)
        if file:# and allowed_file(file.filename):
            filename = secure_filename(file.filename+'lolokljlajfdl')
            print('done', filename)
            file.save(os.path.join(app.config['UPLOAD_FOLDER'], filename))
            return 'file uploaded'
    return '''
    <!doctype html>
    <title>Upload new File</title>
    <h1>Upload new File</h1>
    <form method=post enctype=multipart/form-data>
      <input type=file name=file>
      <input type=submit value=Upload>
    </form>
    '''