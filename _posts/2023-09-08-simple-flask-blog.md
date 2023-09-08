---
layout: post
title:  Simple flask Blog
date:   2023-09-08
categories: Flask
tags: FLASK
---
Great! Let's proceed step-by-step to build the simple Flask blog. Since running the Flask app here isn't possible, you can copy the code into your local environment to run it.

### File Structure

Here's what the file structure should look like:

```
my_blog/
│
├── app.py
└── templates/
    ├── index.html
    ├── post.html
    └── add.html
```

### 1. `app.py`

Here's the complete code for `app.py`:

```python
from flask import Flask, render_template, request, redirect

app = Flask(__name__)

# In-memory storage for blog posts
posts = []

@app.route('/')
def index():
    return render_template('index.html', posts=posts)

@app.route('/post/<int:post_id>')
def post(post_id):
    post = posts[post_id]
    return render_template('post.html', post=post)

@app.route('/add', methods=['GET', 'POST'])
def add():
    if request.method == 'POST':
        title = request.form['title']
        content = request.form['content']
        posts.append({'title': title, 'content': content})
        return redirect('/')
    return render_template('add.html')

if __name__ == '__main__':
    app.run(debug=True)
```



### Run the App

To run the app, navigate to the `my_blog` folder in your terminal and run:

```bash
python app.py
```

Your Flask blog should now be running at `http://127.0.0.1:5000/`. You can add new posts by going to `http://127.0.0.1:5000/add` and view individual posts by clicking on their titles on the homepage.

Please note that this is a very basic example. For a production environment, you should consider using a real database, adding user authentication, and so on.
