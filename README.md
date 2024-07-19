# Flask Blog Application

This is a simple Flask blog application that fetches posts from an API and displays them. Each post has a title, subtitle, and body content.

## Project Structure

The project consists of the following files:

- `main.py`: The main Flask application file.
- `post.py`: The Post class file used to create post objects.
- `templates/index.html`: The template for the home page.
- `templates/post.html`: The template for individual post pages.
- `static/css/styles.css`: The CSS file for styling the application.

## Requirements

- Python 3.6+
- Flask
- Requests

## Installation

1. Clone the repository:
   ```bash
   git clone https://github.com/Prathamesh326/blog-templating-using-flask.git
   ```
2. Navigate to the project directory:
   ```bash
   cd blog-templating-using-flask
   ```
3. Create a virtual environment:
   ```bash
   python -m venv venv
   ```
4. Activate the virtual environment:
   - On Windows:
     ```bash
     venv\Scripts\activate
     ```
   - On macOS and Linux:
     ```bash
     source venv/bin/activate
     ```
5. Install the required packages:
   ```bash
   pip install -r requirements.txt
   ```

## Usage

1. Run the Flask application:
   ```bash
   python main.py
   ```
2. Open your browser and navigate to `http://127.0.0.1:5000/` to view the blog.

## Project Files

### main.py

```python
from flask import Flask, render_template
from post import Post
import requests

posts = requests.get("https://api.npoint.io/c790b4d5cab58020d391").json()
post_objects = []

for post in posts:
    post_obj = Post(post['id'], post['title'], post['subtitle'], post['body'])
    post_objects.append(post_obj)

    print(post_objects)

app = Flask(__name__)

@app.route('/')
def get_all_posts():
    return render_template("index.html", all_posts=post_objects)

@app.route("/post/<int:index>")
def show_post(index):
    requested_post = None
    for blog_post in post_objects:
        if blog_post.id == index:
            requested_post = blog_post
    return render_template("post.html", post=requested_post)

if __name__ == "__main__":
    app.run(debug=True)
```

### post.py

```python
class Post:
    def __init__(self, post_id, title, subtitle, body):
        self.id = post_id
        self.title = title
        self.subtitle = subtitle
        self.body = body
```

### templates/index.html

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <link href="https://fonts.googleapis.com/css2?family=Raleway" rel="stylesheet">
    <link rel="stylesheet" href="../static/css/styles.css">
</head>
<body>
<div class="wrapper">
    <div class="top">
        <div class="title"><h1>My Blog</h1></div>
    </div>

    {%for post in all_posts:%}
    <div class="content">
        <div class="card">
            <h2>{{ post.title }}</h2>
            <p class="text"> {{post.subtitle}} </p>
            <a href="{{url_for('show_post', index=post.id)}}">Read</a>
        </div>
    </div>
    {% endfor %}

</div>
</body>
<footer>
    <p>Made with ♥️ in London.</p>
</footer>
</html>
```

### templates/post.html

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <link href="https://fonts.googleapis.com/css2?family=Raleway" rel="stylesheet">
    <link rel="stylesheet" href="../static/css/styles.css">
</head>
<body>
<div class="wrapper">
    <div class="top">
        <div class="title"><h1>My Blog</h1></div>
           </div>
        <div class="content">
            <div class="card">
                <h1>{{post.title}}</h1>
                <h2>{{post.subtitle}}</h2>
                <p>{{post.body}}</p>
            </div>

    </div>
</div>
</body>
<footer>
    <p>Made with ♥️ in London.</p>
</footer>
</html>
```

### static/css/styles.css

```css
body{
 background: #efeff3;
 margin: 0;
 font-family: 'Raleway', sans-serif;
 -webkit-font-smoothing: antialiased;
  color:#212121;
}
.wrapper{
  position: relative;
  clear:both;
  margin: 0 auto 75px auto;
  width: 100%;
  overflow: hidden;
}
.top{
  background: #4e89ae;
  height: 180px;
  border-top: 20px solid #43658b;
}

.top .title {
  width: 700px;
  margin: 38px auto 0 auto; 
}

.title h1 {
  font-size:24px;
  color:#FFF;
  font-weight:500;
}

.content{
    margin: -80px auto 100px;
  padding-bottom: 20px;
}

.card{
  position: relative;
  background: #fff;
  padding:50px;
  width: 600px;
  margin: 20px auto 0 auto;
  box-shadow: 0 2px 4px rgba(100,100,100,.1);
}

.card h2 {
  font-size:21px;
  font-weight:500;
}

.card h2 a {
  color:#CC0000;
  text-decoration:none;
}

.card .text {
  color:#212121;
  margin-top:20px;
  font-size:15px;
  line-height:22px;
}

footer {
  position: fixed;
  left: 0;
  bottom: 0;
  width: 100%;
  background-color: #43658b;
  color: white;
  text-align: center;
}
```

## API

The posts are fetched from the following API:
```
https://api.npoint.io/c790b4d5cab58020d391
```
