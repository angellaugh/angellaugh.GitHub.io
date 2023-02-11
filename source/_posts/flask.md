---
layout: posts
title: flask
date: 2023-02-11 13:30:46
updated: 2023-02-11 13:31:43
tags: [flask]
categories: Teckknowledge
---

# flask

FLASK_APP=/usr/local/devcodes/powerload-web FLASK_ENV=development python3 -m flask run --host=10.219.61.224  --port=2222

<font color=#063060 size=2>1. 命令行启动 flask：先export FLASK_APP指向启动file.py；再flask run;
2. 命令行启动debug模式，export FLASK_DEBUG=1； 此模式可changes reloaded authmatically.
3. python 启动 flask,  if __name__ == '__main__'，可在其中设置debug模式
**4. 用装饰器 app.route("/")  route decorators 来管理页面路径 what we type into the browsing to go to different pages.**</font>


17. login logout, 验证注册是否使用存在了的username 或者email。 登录后不再显示login 和 注册按钮，
只显示logout按钮。 logout之后，不可以再访问account页面。  首次登陆直接展示account页面，再次登录直接展示home页面。

##### 先做点简单的
`pip install flask`
`mkdir Flask_blog`
`touch flasklog.py`
```python
from flask import Flask
app = Flask(__name__)

@app.route('/')  # root page
def hello_world():
    return 'Hello, World!'
```
---
##### 增加venv

1. `export FLASK_APP=flaskblog.py`
2. flask run

**只要做了改动，就要restart flask，才能生效**
3. 开启debug模式： `export FLASK_DEBUG=1`，就可以实时更新改动:thoes changes reload automatically.

4. 增加  main 之后，用python来run
    1. `python flaskblog.py`
   
```python
from flask import Flask
app = Flask(__name__)

<!--more-->

@app.route('/')  # root page, two routes are handled by the same function
@app.route('/home')
def home():
    return '<h1>Hello, World!</h1>' 



@app.route('/about')  # root page
def about():
    return '<h1>About Page!</h1>' 

if __name__ == "__main__":
    app.run(debug=True)
```

---


5. 在layout.html中，包含网站主要css样式，其中，body中添加 block模块，可以用来让
the block section that the child templates can override. 调用它的其他html可以重写覆盖。


6. mkdir static 来存放 css样式， main.css
7. 从 snippets中复制过来的navigation.html样式，放在了body标签里面；
`https://github.com/CoreyMSchafer/code_snippets/blob/master/Python/Flask_Blog/snippets/navigation.html`
8. 添加main.html 到layout.html的 body中 head下面：
9. 在layout.html的head中，title前面，添加link，来引用static中的css样式
    1. 注意 flaskblog.py 中 import ulr_for: `from flask import Flask, render_template, url_for`
    2. 改写layout.html，增加如下：
`<link rel="stylesheet" type="text/css" href="{{ url_for('static', filename='main.css') }}">`

---

10. 安装flask-wtf： `pip install flask-wtf`


11. register.html中 各种div 之内的 form.username/password/confirm_password都来自于 forms.py中 class 中定义的变量名字

12. 但是最后一个div中， 指的是flaskblog.py中的fun name:
@app.route('/login')
def <font color=#FF0000>login():</font>

```html
    <div class="border-top pt-3">
        <small class="text-muted">
            Already Have An Account? <a class="ml-2"  href="{{ url_for('login') }}"></a>
        </small>
    </div>
```
13. flash :  redirect  --- flasklog.py
在register中，添加，如果submit验证成功，就返回到home page ，这个home，
调用的是 flasklog.py中的 home fun
flash message 是一次性的，当再次刷新时，因为未触发到那个条件，所以flash message不再出现，除非再次注册。

加入flash 和 redirect 来控制，做了注册之后，页面的刷新跳转；

```python
@app.route('/')  # root page, two routes are handled by the same function
@app.route('/home')
def home():
    return render_template('home.html', posts=posts)

@app.route('/about')  # root page
def about():
    return render_template('about.html', title='About')

@app.route('/register', methods=['GET', 'POST'])  
def register():
    form = RegistrationForm()
    if form.validate_on_submit():
        flash(f'Account created for {form.username.data}!', 'success')
        return redirect(url_for('home'))
    return render_template('register.html', title='Register', form=form)


```
13.2  但是当register 失败后，并无feedback 来告诉用户  why it was invalid

添加了一些验证在 register.html， 用的是如果拿到error信息就展示出来

![](_v_images/20201019234332261_1025641223.png =300x)


14. 在login 加了 flash信息 ，hardcode写了 用户名和密码
![](_v_images/20201020002527262_100800492.png =300x)


---

15. `pip install flask-sqlalchemy`

~~export  PIP_DEFAULT_TIMEOUT=100~~  无效
sudo pip install --default-timeout=100 future
pip install --default-timeout=220 future
因为sudo 调用的pip 是3.6

![](_v_images/20201021220751159_1841048205.png =600x)



python
```python
from flaskblog import db
db.create_all()    ---> site.db file

from flaskblog import User, Post
user_1 = User(username='Corey', email='C@demo.com', password='password')

db.session.add(user_1)
user_2 = User(username='johnDone', email='jd@demo.com', password='password')

db.session.add(user_2)


db.session.commit()

User.query.all()

User.query.first()

User.query.filter_by(username='Corey').all()


User.query.filter_by(username='Corey').first()

user = User.query.filter_by(username='Corey').first()

user.id
user = User.query.get(1)
user

user.posts

post_1 = Post(title='Blog 1 ', content = 'first post content!', user_id = user.id)

post_2 = Post(title='Blog 2 ', content = 'Second post content!', user_id = user.id)

db.session.add(post_1)

db.session.add(post_2)

db.session.commit()

user.posts
for post in user.posts:
    print(post.title)

post = Post.query.first()

post.user_id
post.author

db.drop_all()
db.create_all()
User.query.all()
Post.query.all()
```


---

16. `pip install flask-bcrypt`
用bcrypt 来生成hash 加密，每次生成都不一样，
但是反之check却可以验证真伪。
```python
~ 🔥» python                      vivi@vivideMacBook-Pro
Python 3.8.5 (v3.8.5:580fbb018f, Jul 20 2020, 12:11:27)
[Clang 6.0 (clang-600.0.57)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>> from flask_bcrypt import Bcrypt
>>> bcrypt = Bcrypt()
>>> bcrypt.generate_password_hash('testing')
b'$2b$12$mwleE8U2426VUFW1ZfjI0OsrWatmzYla/hCHhiH5t/xsLxQl/ob6i'
>>> bcrypt.generate_password_hash('testing').decode('utf-8')
'$2b$12$6kcDKHgJJDhlwaZZV9l0f.DPEqxcJFx01axApkdAVm/v5uoiQKyHe'
>>> bcrypt.generate_password_hash('testing').decode('utf-8')
'$2b$12$8LMjQhBOdUqT.ZLF08SreOf7CrOYCtgOaEEXHal5okuXclqgviR1e'
>>> hashed_pw = bcrypt.generate_password_hash('testing').decode('utf-8')
>>> bcrypt.check_password_hash(hashed_pw,"password")
False
>>> bcrypt.check_password_hash(hashed_pw,"testing")
True
>>>
```

添加了一些校验之后，在首页注册成功

```python
~/Desktop/Flask_Blog 🔥» python   vivi@vivideMacBook-Pro
Python 3.8.5 (v3.8.5:580fbb018f, Jul 20 2020, 12:11:27)
[Clang 6.0 (clang-600.0.57)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>> from flaskblog import db
/Library/Frameworks/Python.framework/Versions/3.8/lib/python3.8/site-packages/flask_sqlalchemy/__init__.py:833: FSADeprecationWarning: SQLALCHEMY_TRACK_MODIFICATIONS adds significant overhead and will be disabled by default in the future.  Set it to True or False to suppress this warning.
  warnings.warn(FSADeprecationWarning(
>>> from flaskblog.models import User
>>> user = User.query.first()
>>> user
User('ali1', 'ali1@flask.com', 'default.jpg')
>>> user.password
'$2b$12$JM7NPMUz6329UQmMtj8p7uDFWtKRHmpQujnffwW0U/W1WNOfWHThG'
>>>
```

 用已经存在的用户来注册，页面报错
```python
sqlalchemy.exc.IntegrityError
sqlalchemy.exc.IntegrityError: (sqlite3.IntegrityError) UNIQUE constraint failed: user.email
[SQL: INSERT INTO user (username, email, image_file, password) VALUES (?, ?, ?, ?)]
[parameters: ('ali1', 'ali1@flask.com', 'default.jpg', '$2b$12$HjaXxkdNbmjhLSLTu7CSsO9Nx/HLJG8.1cUrBL9TwS79/Zui9wvSm')]
(Background on this error at: http://sqlalche.me/e/13/gkpj)
```


在浏览器页面，到某个地方想执行，输入pin码即可调用python生成各种信息
所以，不可以生产环境使用debug模式，暴露的信息太多。
![](_v_images/20201022210758556_1552795459.png =500x)![](_v_images/20201022210936423_1612001069.png =500x)

添加了一些验证之后：
![](_v_images/20201022212003059_248276131.png =500x)




17. `pip install flask-login`  安装login 模块



----


User Account and Profile Picture


