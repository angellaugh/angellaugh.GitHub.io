---
layout: posts
title: diango
date: 2023-02-11 13:43:46
#updated: 2023-02-11 13:31:43
tags: django 
categories: Teckknowledge
password: lantian

---

# 1. django

1. pip3.8 install djangon
2. python3.8 -m django --version
3. django-admin
4. django-admin startproject  ~~myprojectname~~  Multicorn_project  | on desktop
5. cd ~~myprojectname~~ Multicorn_project
6. tree
![](_v_images/20201001165628417_1726437634.png =200x)

7. python manage.py runserver
```
October 01, 2020 - 08:57:22
Django version 3.1.2, using settings 'Multicorn_project.settings'
Starting development server at http://127.0.0.1:8000/
```
8. python manage.py runserver 9999   | grep 端口
9. python manage.py startapp ~~yourappname~~ mouticorn

10. dirs:   multicorn -> templates -> multicorn -> 2 files: home.html  and multicornjson.html
11. in home.html,  just input "html + tab"
12. look at the apps.py in the multicorn app 
13. in project settings.py,  add multicorn.apps.MulticornConfig into the top of 'INSTALLED_APPS'
![](_v_images/20201003160503076_808966192.png =320x)

14. dirs: multicorn -> static -> multicorn   
<!--more-->
15. python manage.py makemigrations
16. python manage.py migrate
17. 
16.  python manage.py createsuperuser  创建一个 admin user
username: ali1
password: lantian
email: annie.li1@dell.com

```
/Desktop/Multicorn_project 🔥» python manage.py createsuperuser                                                                      vivi@vivideMacBook-Pro
Username (leave blank to use 'vivi'): ali1
Email address: annie.li1@dell.com
Password:
Password (again):
This password is too short. It must contain at least 8 characters.
Bypass password validation and create user anyway? [y/N]: y
Superuser created successfully.
```

> [一个有用的css网站](https://getbootstrap.com/docs/4.5/getting-started/introduction/)
18.  在base.css中，把Login的 href 改成 **href="/admin/"**, 即可登录
![](_v_images/20201004152628847_575819787.png =520x)

username: TestUser   password: django123

![](_v_images/20201004154036210_1241846727.png =220x)

19. models.py


```pythonx
from django.db import models
from django.utils import timezone
from django.contrib.auth.models import User

class Post(models.Model):
    title = models.CharField(max_length=100)
    content = models.TextField()
    # auto_now, auto_now_add, default
    date_posted = models.DateTimeField(default=timezone.now)
    author = models.ForeignKey(User, on_delete=models.CASCADE)
```


20. python manage.py makemigrations    muticorn -> migrations -> 0001_initial.py & __init__.py
在 0001_initial.py中是 建表语句

```
~/Desktop/Multicorn_project 🔥» python manage.py makemigrations                                                                       vivi@vivideMacBook-Pro
Migrations for 'multicorn':
  multicorn/migrations/0001_initial.py
    - Create model Post
```

21.  python manage.py sqlmigrate multicorn 0001_initial  
执行建表语句并且commit 到db中

```
~/Desktop/Multicorn_project 🔥» python manage.py sqlmigrate multicorn 0001_initial                                                    vivi@vivideMacBook-Pro
BEGIN;
--
-- Create model Post
--
CREATE TABLE "multicorn_post" ("id" integer NOT NULL PRIMARY KEY AUTOINCREMENT, "title" varchar(100) NOT NULL, "content" text NOT NULL, "date_posted" datetime NOT NULL, "author_id" integer NOT NULL REFERENCES "auth_user" ("id") DEFERRABLE INITIALLY DEFERRED);
CREATE INDEX "multicorn_post_author_id_82a8e3b2" ON "multicorn_post" ("author_id");
COMMIT;
```
22. python manage.py migrate
```
~/Desktop/Multicorn_project 🔥» python manage.py migrate                                   vivi@vivideMacBook-Pro
Operations to perform:
  Apply all migrations: admin, auth, contenttypes, multicorn, sessions
Running migrations:
  Applying multicorn.0001_initial... OK
```

23. 进入 django shell 模式
24. python manage.py shell

```python
from multicorn.models import Post
from django.contrib.auth.models import User
User.objects.all()

>> <QuerySet [<User: ali1>, <User: TestUser>]>
```

可以在shell模式下，筛选
    1. User.objects.all()        -->     <QuerySet [<User: ali1>, <User: TestUser>]>
    2. User.objects.first()         -->           <User: ali1>
    3. User.objects.filter(username='ali1')            -->               <QuerySet [<User: ali1>]>
    4. User.objects.filter(username='ali1').first()      -->           <User: ali1>

可以 赋值 user 做各种操作：
    1. user = User.objects.filter(username='ali1').first() 
        ![](_v_images/20201004221441020_1148517328.png =300x)
    2. user.id 
    3. user.pk
    4. user = User.objects.get(id=1)
    5. user   --->   <User: ali1>
    ![](_v_images/20201004222758993_1547712460.png =200x)
ctrl + l  python中的清屏

可以给新表 Post 增加数据：
```python
>>> Post.objects.all()
<QuerySet []>
>>> post_1 = Post(title='blog 1', content='First Post Content!', author=user)
>>> Post.objects.all()
<QuerySet []>
>>> post_1.save()
>>> Post.objects.all()
<QuerySet [<Post: Post object (1)>]>
```
25. 因为打印出来的 Post.objects.all()  没有展示title
进入 multicorn -> models.py  在class Post中添加
def __str__(self):
        return self.title

```python
class Post(models.Model):
    title = models.CharField(max_length=100)
    content = models.TextField()
    # auto_now, auto_now_add, default
    date_posted = models.DateTimeField(default=timezone.now)
    author = models.ForeignKey(User, on_delete=models.CASCADE)

    def __str__(self):
        return self.title
```


26. 重新打开shell：
```python
python manage.py shell
>>> from multicorn.models import Post
>>> from django.contrib.auth.models import User
>>> Post.objects.all()
<QuerySet [<Post: blog 1>]>
```

27. 添加第二个blog,  注意`post_2.save()`, 注意 `author_id = user.id`

```python
>>> user = User.objects.filter(username='ali1').first()
>>> user
<User: ali1>
>>> post_2 = Post(title='Blog 2', content='Second Post Content!', author=user)
>>> post_2 = Post(title='Blog 2', content='Second Post Content!', author_id=user.id)
>>> post_2.save()
>>> Post.objects.all()
<QuerySet [<Post: blog 1>, <Post: Blog 2>]>
>>>
```
28. 可以查看post的各种属性，包括 `date_posted`,  和 `内建的user`, 和 `email`
```python
>>> Post.objects.all()
<QuerySet [<Post: blog 1>, <Post: Blog 2>]>
>>> post = Post.objects.first()
>>> post.content
'First Post Content!'
>>> post.date_posted
datetime.datetime(2020, 10, 4, 14, 30, 35, 354395, tzinfo=<UTC>)
>>> post.author
<User: ali1>
>>> post.author.email
'annie.li1@dell.com'
```
29. 可以查看同一个user的 所有 Post


```python
>>> user.post_set
<django.db.models.fields.related_descriptors.create_reverse_many_to_one_manager.<locals>.RelatedManager object at 0x7ff829552640>
>>> user.post_set.all()
<QuerySet [<Post: blog 1>, <Post: Blog 2>]>
```
30. 可以用 user 来create 第三个blog，并且，无需像调用Post那样 `调用save来保存`

```python
>>> user.post_set.all()
<QuerySet [<Post: blog 1>, <Post: Blog 2>]>
>>> user.post_set.create(title='Blog 3', content='Third Post Content!')
<Post: Blog 3>
>>> user.post_set.all()
<QuerySet [<Post: blog 1>, <Post: Blog 2>, <Post: Blog 3>]>
>>> Post.objects.all()
<QuerySet [<Post: blog 1>, <Post: Blog 2>, <Post: Blog 3>]>
```
31. 回到views里改代码，把dummy data 换成 DB中的Post， 然后傻眼了哈哈哈
因为，我之前dummy data 用的字段是工作中的， 和templates 里的 html 兼容
现在要回到， home 中，把  for post in posts_parameters_key 下的字段都换成与Post表一致的内容

**views.py**

```python
def home(request):
    context = {
        # 把 嵌套在views.py中的dummy data posts 换成了DB data Post.objects.all()
        #'posts_parameters_key': posts
        'posts_parameters_key': Post.objects.all()
    }
    return render(request, 'multicorn/home.html', context)

```
31. 重新启动网站， python manage.py runserver 9999
![](_v_images/20201004231145556_2081382756.png =300x)

31.1 

```html
<small class="text-muted">{{ post.date_posted | date:"F d, Y" }}</small>
```
![](_v_images/20201004231458499_7661096.png =200x)

```html
<small class="text-muted">{{ post.date_posted }}</small>
```

![](_v_images/20201004231410304_1438854109.png =200x)


32.  进入admin.py 注册Post， 这样就可以在admin 页面看到Post的 blog了

```python
from django.contrib import admin
from .models import Post

admin.site.register(Post)
```
 
![](_v_images/20201004233353387_2058505299.png =300x)

变更一下 blog3的 author， 回到multicorn/home中，发现作者已变
![](_v_images/20201004233623918_1337007123.png =200x)
