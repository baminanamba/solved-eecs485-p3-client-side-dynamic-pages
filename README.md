Download Link: https://assignmentchef.com/product/solved-eecs485-p3-client-side-dynamic-pages
<br>
<h1>Introduction</h1>

An Instagram clone implemented with client-side dynamic pages. This is the third of an EECS 485 three project sequence: a static site generator from templates, server-side dynamic pages, and client-side dynamic pages.

Build an application using client-side dynamic pages and a REST API. Reuse server-side code from project 2, refactoring portions of it into a REST API. Write a client application in JavaScript that runs in the browser and makes AJAX calls to the REST API.

The learning goals of this project are client-side dynamic pages, JavaScript programming, asynchronous programming (AJAX), and REST APIs. You’ll also gain more practice with the command line.

This spec will walk you through several parts:

<ol>

 <li>Setup</li>

 <li>REST API Specification</li>

 <li>Client-side Insta485 specification</li>

 <li>Testing</li>

 <li>Deploy to AWS</li>

 <li>Submitting and grading</li>

 <li>FAQ</li>

</ol>

<h1>Setup</h1>

<strong>Group registration</strong>

Register your group on the <a href="https://autograder.io/">Autograder</a>.

<h2>AWS account and instance</h2>

You will use Amazon Web Services (AWS) to deploy your project. AWS account setup may take up to 24 hours, so get started now. Create an account, launch and configure the instance. Don’t deploy yet. <a href="https://eecs485staff.github.io/p2-insta485-serverside/setup_aws.html">AWS Tutorial</a><a href="https://eecs485staff.github.io/p2-insta485-serverside/setup_aws.html">.</a>

<h2>Project folder</h2>

Create a folder for this project (<a href="https://eecs485staff.github.io/p1-insta485-static/setup_os.html#create-a-folder">instructions</a><a href="https://eecs485staff.github.io/p1-insta485-static/setup_os.html#create-a-folder">)</a>. Your folder location might be different.

<strong>WARNING:</strong> avoid file paths containing spaces in the project. Spaces can cause problems with the local tool installations.

$ pwd

/Users/awdeorio/src/eecs485/p3-insta485-clientside

<h2>Version control</h2>

Set up version control using the <a href="https://eecs485staff.github.io/p1-insta485-static/setup_git.html">Version control tutorial</a>.

Be sure to check out the <a href="https://eecs280staff.github.io/p1-stats/setup_git.html#version-control-for-a-team">Version control for a team</a> tutorial.

After you’re done, you should have a local repository with a “clean” status and your local repository should be connected to a remote GitLab repository.

$ pwd

/Users/awdeorio/src/eecs485/p3-insta485-clientside

$ git status

On branch master

Your branch is up-to-date with ‘origin/master’.

nothing to commit, working tree clean

$ git remote -v

origin https://gitlab.eecs.umich.edu/awdeorio/p3-insta485-clientside.git (fetch) origin https://gitlab.eecs.umich.edu/awdeorio/p3-insta485-clientside.git (push)

You should have a <sub>.</sub><sub>gitig</sub><sub>nore</sub> file (<a href="https://eecs485staff.github.io/p3-insta485-clientside/setup_git.html#add-a-gitignore-file">instructions</a>).

$ pwd

/Users/awdeorio/src/eecs485/p3-insta485-clientside

$ head .gitignore

This is a sample .gitignore file that’s useful for EECS 485 projects.

<em>… </em>

<h2>Python virtual environment</h2>

Create a Python virtual environment using the Project 1 <a href="https://eecs485staff.github.io/p1-insta485-static/setup_virtual_env.html">Python Virtual Environment Tutorial</a>.

Check that you have a Python virtual environment, and that it’s activated (remember <sub>source </sub>env/bin/activate ).

$ pwd

/Users/awdeorio/src/eecs485/p3-insta485-clientside

$ ls -d env env

$ echo $VIRTUAL_ENV

/Users/awdeorio/src/eecs485/p3-insta485-clientside/env

<h2>Install utilities</h2>

<strong>Linux and Windows 10 Subsystem for Linux</strong>

$ sudo apt-get install sqlite3 curl httpie

<strong>MacOS</strong>

$ brew install sqlite3 curl httpie coreutils

<h2>Starter files</h2>

Download and unpack the starter files.

$ pwd

/Users/awdeorio/src/eecs485/p3-insta485-clientside

$ wget https://eecs485staff.github.io/p3-insta485-clientside/starter_files.tar.gz $ tar -xvzf starter_files.tar.gz

Move the starter files to your project directory and remove the original <sub>starter_files/ </sub>directory and tarball.

$ pwd

/Users/awdeorio/src/eecs485/p3-insta485-clientside

$ mv starter_files/<strong>*</strong> .

$ mv starter_files/.eslintrc.js .

$ rm -rf starter_files starter_files.tar.gz You should see these files.

$ tree –matchdirs -I ‘env|__pycache__’

<em>. </em>

├── VERSION

├── bin

│   └── insta485test-html

├── package-lock.json

├── package.json

├── setup.py

├── sql

│   └── uploads

<em>        … </em>

│       └── e1a7c5c32973862ee15173b0259e3efdb6a391af.jpg ├── tests

<em>    … </em>

│   └── util.py

└── webpack.config.js

Here’s a brief description of each of the starter files.

<table width="499">

 <tbody>

  <tr>

   <td width="195">VERSION</td>

   <td width="304">Version of the starter files</td>

  </tr>

  <tr>

   <td width="195">bin/insta485test-html</td>

   <td width="304">Script to test HTML5 compliance</td>

  </tr>

  <tr>

   <td width="195">package-lock.json</td>

   <td width="304">JavaScript packages with dependencies</td>

  </tr>

  <tr>

   <td width="195">package.json</td>

   <td width="304">JavaScript packages</td>

  </tr>

  <tr>

   <td width="195">setup.py</td>

   <td width="304">Insta485 python package configuration</td>

  </tr>

  <tr>

   <td width="195">sql/uploads/</td>

   <td width="304">Sample image uploads</td>

  </tr>

  <tr>

   <td width="195">tests/</td>

   <td width="304">Public unit tests</td>

  </tr>

  <tr>

   <td width="195">webpack.config.js</td>

   <td width="304">JavaScript bundler config</td>

  </tr>

 </tbody>

</table>

Before making any changes to the clean starter files, it’s a good idea to make a commit to your Git repository.

<h2>Copy project 2 code</h2>

You’ll reuse much of your code from project 2. Copy these files and directories from project 2 to project 3:

Scripts bin/insta485db bin/insta485run bin/insta485test

Server-side version of Insta485

insta485/

Database SQL files sql/schema.sql sql/data.sql <strong>Do not</strong> copy:

Virtual environment files <sub>env/</sub>

Python package files insta485.egg-info/ Your directory should now look like this:

$ pwd

/Users/awdeorio/src/eecs485/p3-insta485-clientside $ tree –matchdirs -I ‘env|__pycache__’

<em>. </em>

├── bin

│   ├── insta485db

│   ├── insta485run

│   ├── insta485test

│   └── insta485test-html

├── insta485

│   ├── __init__.py

│   ├── api

│   │   ├── __init__.py

<em>        … </em>

│   ├── config.py

│   ├── model.py

│   ├── static

│   │   ├── css

│   │   │   └── style.css

│   │   ├── images

│   │   │   └── logo.png

│   │   └── js

│   │       └── bundle.js │   ├── templates

<em>        … </em>

│   │   └── index.html

│   └── views

│       ├── __init__.py

├── package-lock.json

├── package.json

├── setup.py

├── sql

│   ├── data.sql

│   ├── schema.sql │   └── uploads

<em>        … </em>

│       └── e1a7c5c32973862ee15173b0259e3efdb6a391af.jpg ├── tests

<em>    … </em>

│   └── util.py

└── webpack.config.js

Use pip to install the <sub>insta485</sub> package.

$ pip install -e .

Run your project 2 code and make sure it still works by navigating to <a href="http://localhost:8000/">http://localhost:8000/</a>.

$ ./bin/insta485db reset

$ ./bin/insta485run

Commit these changes and push to your Git repository.

<h2>REST API</h2>

The <a href="https://eecs485staff.github.io/p2-insta485-serverside/setup_flask.html#rest-api">Flask REST API Tutorial</a> will show you how to create a small REST API with Python/Flask.

Run the Flask development server.

$ ./bin/insta485run

Navigate to <a href="http://localhost:8000/api/v1/p/1/likes/">http://localhost:8000/api/v1/p/1/likes/</a>. You should see this JSON response:

{

<strong>“logname_likes_this”</strong>: 1,

<strong>“likes_count”</strong>: 3,

<strong>“postid”</strong>: 1,

<strong>“url”</strong>: “/api/v1/p/1/likes/”

}

Commit these changes and push to your Git repository.

<h2>REST API tools</h2>

The <a href="https://eecs485staff.github.io/p3-insta485-clientside/setup_restapi_tools.html">REST API Tools Tutorial</a> will show you how to use <sub>curl</sub> and HTTPie (the <sub>http</sub> command) to test a REST API from the command line.

You should now be able to log in and make a REST API call via the command line. Login using HTTPie to fill out the login form. This will save a file called <sub>session.json</sub> containing a cookie set by the server.

$ http 

–session<strong>=</strong>./session.json 

–form POST 

“http://localhost:8000/accounts/login/”    username<strong>=</strong>awdeorio    password<strong>=</strong>password    submit<strong>=</strong>login HTTP/1.0 302 FOUND <em>… </em>

$ http 

–session<strong>=</strong>./session.json 

“http://localhost:8000/api/v1/p/1/likes/” HTTP/1.0 200 OK

<em>… </em>

{

“likes_count”: 3,

“logname_likes_this”: 1,

“postid”: 1,

“url”: “/api/v1/p/1/likes/”

}

<h2>React/JS</h2>

The <a href="https://eecs485staff.github.io/p3-insta485-clientside/setup_react.html">React/JS Tutorial</a> will get you starter with a “hello world” React app and development toolchain.

After completing the tutorial, you have local JavaScript libraries and tools installed. Your versions may be different.

$ ls -d node_modules node_modules

$ echo $VIRTUAL_ENV

/Users/awdeorio/src/eecs485/p3-insta485-clientside/env

$ which npm

/Users/awdeorio/src/eecs485/p3-insta485-clientside/env/bin/npm

$ npm –version

6.5.0

$ which node

/Users/awdeorio/src/eecs485/p3-insta485-clientside/env/bin/node

$ node –version v11.9.0

More tools written in JavaScript were installed via <sub>npm </sub>.

$ ./node_modules/.bin/webpack –version

3.6.0

$ ./node_modules/.bin/eslint –version v4.8.0

You can check the style of your code using <sub>eslint </sub>.

$ ./node_modules/.bin/eslint –ext jsx insta485/js/

Build the front end using <sub>webpack</sub> and then start a Flask development server.

$ ./node_modules/.bin/webpack

$ ./bin/insta485run

Browse to <a href="http://localhost:8000/">http://localhost:8000/</a> where you should see the test “Likes” React Component.

Commit these changes and push to your Git repository.

<h2>End-to-end testing</h2>

The <a href="https://eecs485staff.github.io/p3-insta485-clientside/setup_endtoend_testing.html">End-to-end Testing Tutorial</a> describes how to test a website implemented with client-side dynamic pages.

After completing the tutorial, you should have Google Chrome and Chrome Driver installed. The first part of version should match ( <sub>77</sub> in this example). While your versions should match, they might be different than this example.

$ google-chrome –version         <em># macOS</em>

$ google-chrome-stable –version  <em># WSL/Linux</em>

Google Chrome 77.0.3865.90

$ chromedriver –version

ChromeDriver 77.0.3865.40

Run an end-to-end test provided with the starter files.

$ pytest -v tests/test_index.py::TestIndex::test_react_load

<em>… </em>

::TestIndex::test_react_load PASSED                                       [100%]




=========================== 1 passed in 2.47 seconds ===========================

<h2>Install script</h2>

Installing the tool chain requires a lot of steps! Write a bash script <sub>bin/insta485install</sub> to install your app. Don’t forget to check for <a href="https://eecs485staff.github.io/p1-insta485-static/setup_scripting.html#shell-script-pitfalls">shell script pitfalls</a>.

Remember the shebang

#!/bin/bash

Stop on errors, print commands

set -Eeuo pipefail set -x

Create a Python virtual environment

python3 -m venv env

Activate Python virtual environment, avoiding some bad bash habits in the auto-generated activate script.

set +u

source env/bin/activate set -u

Install nodeenv

pip install nodeenv

Install JavaScript virtual environment

nodeenv –python-virtualenv

Deactivate and reactivate the Python virtual environment

set +u deactivate

source env/bin/activate set -u

Install the latest Chromedriver. Automatically download either the macOS or Linux version.

mkdir -p <strong>${</strong>VIRTUAL_ENV<strong>}</strong>/tmp pushd <strong>${</strong>VIRTUAL_ENV<strong>}</strong>/tmp

CHROMEDRIVER_VERSION<strong>=</strong>`curl https://chromedriver.storage.googleapis.com/LATEST_RELEASE` CHROMEDRIVER_ARCH<strong>=</strong>linux64

<strong>if</strong> <strong>[</strong> `uname -s` <strong>=</strong> “Darwin” <strong>]</strong>; <strong>then   </strong>CHROMEDRIVER_ARCH<strong>=</strong>mac64 <strong>fi </strong>wget https://chromedriver.storage.googleapis.com/<strong>${</strong>CHROMEDRIVER_VERSION<strong>}</strong>/chromedriver_ unzip chromedriver_<strong>${</strong>CHROMEDRIVER_ARCH<strong>}</strong>.zip mv chromedriver <strong>${</strong>VIRTUAL_ENV<strong>}</strong>/bin/ popd

pip install -e .

Install front end

npm install .

Remember to add <sub>bin/insta485install</sub> to your Git repo and push.

<strong>WARNING</strong> Do <strong>not</strong> commit automatically generated or binary files to your Git repo! They can cause problems when running the code base on other computers, e.g., on AWS or a group member’s machine. These should all be in your <sub>.</sub><sub>gitig</sub><sub>nore </sub>.

env/ insta485.egg-info node_modules __pycache__ bundle.js tmp cookies.txt session.json var

<h2>Fresh install</h2>

These instructions are useful for a group member installing the toolchain after checking out a fresh copy of the code.

Check out a fresh copy of the code and change directory.

$ git clone &lt;your git URL here&gt;

$ cd p3-insta485-clientside/

If you run into trouble with packages or dependencies, you can delete these automatically generated files.

$ pwd

/Users/awdeorio/src/eecs485/p3-insta485-clientside

$ rm -rf env/ node_modules/ insta485.egg-info/ insta485/static/js/bundle.js Run the installer created during the setup tutorial.

$ ./bin/insta485install

Activate the newly created virtual environment.

$ source env/bin/activate

That’s it!

<h1>Database</h1>

Use the same database schema and starter data as in the <a href="https://eecs485staff.github.io/p2-insta485-serverside/#database">Project 2 Database instructions</a>.

After copying <sub>data.sql</sub> and <sub>schema.sql</sub> from project 2, your <sub>sql/</sub> directory should look like this.

$ tree sql sql

├── data.sql

├── schema.sql └── uploads

<em>    … </em>

└── e1a7c5c32973862ee15173b0259e3efdb6a391af.jpg

<h2>Database management shell script</h2>

Reuse your same database management shell script ( <sub>insta485db </sub>) from <a href="https://eecs485staff.github.io/p2-insta485-serverside/setup_sqlite.html#database-management-shell-script">project 2</a>. Your script should already support these subcommands:

$ insta485db create

$ insta485db destroy

$ insta485db reset

$ insta485db dump

Add the <sub>insta485db random</sub> subcommand, which will generate 100 posts in the database each with owner <sub>awdeorio</sub> and a random photo (selected from the starter photos).

Here is a bash snippet that adds 100 posts to the database each with owner <sub>awdeorio</sub> and a random photo. <strong>Note</strong>: you will not need to modify this bash snippet, but you will need to add the random subcommand to your bash script.

SHUF<strong>=</strong>shuf

<em># If shuf is not on this machine, try to use gshuf instead </em><strong>if</strong> <strong>!</strong> type shuf 2&gt; /dev/null; <strong>then   </strong>SHUF<strong>=</strong>gshuf <strong>fi </strong>

DB_FILENAME<strong>=</strong>var/insta485.sqlite3

FILENAMES<strong>=</strong>“122a7d27ca1d7420a1072f695d9290fad4501a41.jpg            ad7790405c539894d25ab8dcf0b79eed3341e109.jpg            9887e06812ef434d291e4936417d125cd594b38a.jpg            2ec7cf8ae158b3b1f40065abfb33e81143707842.jpg” <strong>for </strong>i <strong>in</strong> `seq 1 100`; <strong>do</strong>

<em># echo $FILENAMES      print string</em>

<em># shuf -n1             select one random line from multiline input</em>

<em># awk ‘{$1=$1;print}’  trim leading and trailing whitespace</em>




<em># Use ‘${SHUF}’ instead of ‘shuf’</em>

FILENAME<strong>=</strong>`echo “$FILENAMES” | <strong>${</strong>SHUF<strong>}</strong> -n1 | awk ‘{$1=$1;print}’`   OWNER<strong>=</strong>“awdeorio”

sqlite3 -echo -batch <strong>${</strong>DB_FILENAME<strong>}</strong> “INSERT INTO posts(filename, owner) VALUES(‘<strong>${</strong>FILENA <strong>done</strong>

<strong>MacOS:</strong> the <sub>insta485db random</sub> code above using the <sub>shuf</sub> (or <sub>gshuf </sub>) command-line utility, which not installed by default. Install the <sub>coreutils</sub> package, which includes <sub>gshuf </sub>.

$ brew install coreutils

<h1>REST API Specification</h1>

This section describes the REST API implemented by the server. It implements the functionality needed to implement the main insta485 page. You might find <a href="https://blog.miguelgrinberg.com/post/designing-a-restful-api-with-python-and-flask">this tutorial on REST APIs</a> using Python/Flask helpful.

The following table describes each REST API method.

<table width="684">

 <tbody>

  <tr>

   <td width="125"><strong>HTTP Method</strong></td>

   <td width="248"><strong>Example URL</strong></td>

   <td width="311"><strong>Action</strong></td>

  </tr>

  <tr>

   <td width="125">GET</td>

   <td width="248">/api/v1/</td>

   <td width="311">Return API resource URLs</td>

  </tr>

  <tr>

   <td width="125">GET</td>

   <td width="248">/api/v1/p/</td>

   <td width="311">Return 10 newest posts</td>

  </tr>

  <tr>

   <td width="125">GET</td>

   <td width="248">/api/v1/p/?size=N</td>

   <td width="311">Return N newest posts</td>

  </tr>

  <tr>

   <td width="125">GET</td>

   <td width="248">/api/v1/p/?page=N</td>

   <td width="311">Return N’th page of posts</td>

  </tr>

  <tr>

   <td width="125"><strong>HTTP Method</strong></td>

   <td width="248"><strong>Example URL</strong></td>

   <td width="311"><strong>Action</strong></td>

  </tr>

  <tr>

   <td width="125">GET</td>

   <td width="248">/api/v1/p/&lt;postid&gt;/</td>

   <td width="311">Return post metadata: URL, username, etc.</td>

  </tr>

  <tr>

   <td width="125">GET</td>

   <td width="248">/api/v1/p/&lt;postid&gt;/comments/</td>

   <td width="311">Return comments for one post</td>

  </tr>

  <tr>

   <td width="125">POST</td>

   <td width="248">/api/v1/p/&lt;postid&gt;/comments/</td>

   <td width="311">Create comment</td>

  </tr>

  <tr>

   <td width="125">GET</td>

   <td width="248">/api/v1/p/&lt;postid&gt;/likes/</td>

   <td width="311">Return number of likes</td>

  </tr>

  <tr>

   <td width="125">POST</td>

   <td width="248">/api/v1/p/&lt;postid&gt;/likes/</td>

   <td width="311">Create like</td>

  </tr>

  <tr>

   <td width="125">DELETE</td>

   <td width="248">/api/v1/p/&lt;postid&gt;/likes/</td>

   <td width="311">Delete like</td>

  </tr>

 </tbody>

</table>

Reminder: all the following examples require a logged in user. Here’s how to log in and save session cookies using <sub>curl </sub>:

$ curl 

–request POST 

–cookie-jar cookies.txt 

–form ‘username=awdeorio’ 

–form ‘password=password’ 

–form ‘submit=login’ 

“http://localhost:8000/accounts/login/”

<h2>GET /api/v1/</h2>

Return a list of services available. The output should look exactly like this example. Note that curl -b is the same as curl –cookie .

$ curl -b cookies.txt “http://localhost:8000/api/v1/”

{

“posts”: “/api/v1/p/”,

“url”: “/api/v1/”

}

<h2>GET /api/v1/p/</h2>

Return the 10 newest posts. The posts should meet the following criteria: each post is made by a user which the logged in user follows or the post is made by the logged in user. The URL of the next page of posts is returned in <sub>next </sub>. Note that <sub>postid</sub> is an int, not a string.

$ curl -b cookies.txt “http://localhost:8000/api/v1/p/”

{

“next”: “”,

“results”: [

{

“postid”: 3,

“url”: “/api/v1/p/3/”

},

{

“postid”: 2,

“url”: “/api/v1/p/2/”

},

{

“postid”: 1,

“url”: “/api/v1/p/1/”

}

],

“url”: “/api/v1/p/”

}

Request a specific number of results with <sub>?size=N </sub>.

$ curl -b cookies.txt “http://localhost:8000/api/v1/p/?size=1”

{

“next”: “/api/v1/p/?size=1&amp;page=1”,

“results”: [

{

“postid”: 3,

“url”: “/api/v1/p/3/”

}

],

“url”: “/api/v1/p/”

}

Request a specific page of results with <sub>?page=N </sub>.

$ curl -b cookies.txt “http://localhost:8000/api/v1/p/?page=1”

{

“next”: “”,

“results”: [],

“url”: “/api/v1/p/”

}

Put <sub>size</sub> and <sub>page</sub> together.

$ curl -b cookies.txt “http://localhost:8000/api/v1/p/?size=1&amp;page=1”

{

“next”: “/api/v1/p/?size=1&amp;page=2”,

“results”: [

{

“postid”: 2,

“url”: “/api/v1/p/2/”

}

],

“url”: “/api/v1/p/”

}

Both <sub>size</sub> and <sub>page</sub> must be non-negative integers. Hint: let Flask coerce to the integer type in a query string like this: flask.request.args.get(“size”, default=&lt;some number&gt;, type=int) .

$ curl -b cookies.txt “http://localhost:8000/api/v1/p/?page=-1”

{

“message”: “Bad Request”,

“status_code”: 400

}

$ curl -b cookies.txt “http://localhost:8000/api/v1/p/?size=-1”

{

“message”: “Bad Request”,

“status_code”: 400

}

HINT: Use an SQL query with <sub>LIMIT</sub> and <sub>OFFSET </sub>, which you can compute from the <sub>page</sub> and size parameters. NOTE: “age” should not be returned as humanreadble from API.

<strong>Pro-tip:</strong> Returning the newest posts can be tricky due to the fact that all the posts are generated at nearly the same instant. If you tried to order by timestamp, this could potentially cause ‘ties’. Instead of using timestamp, use the fact that post ID is auto incremented in the order of creation to get the correct order.

<h2>GET /api/v1/p/&lt;postid&gt;/</h2>

Return the details for one post. Example:

$ curl -b cookies.txt “http://localhost:8000/api/v1/p/3/”

{

“age”: “2017-09-28 04:33:28”,

“img_url”: “/uploads/9887e06812ef434d291e4936417d125cd594b38a.jpg”,

“owner”: “awdeorio”,   “owner_img_url”: “/uploads/e1a7c5c32973862ee15173b0259e3efdb6a391af.jpg”,

“owner_show_url”: “/u/awdeorio/”,

“post_show_url”: “/p/3/”,

“url”: “/api/v1/p/3/”

}

HINT: <sub>&lt;postid&gt;</sub> must be an integer. Let Flask enforce the integer type in a URL like this:

<strong>@</strong>insta485<strong>.</strong>app<strong>.</strong>route(‘/api/v1/p/&lt;int:postid_url_slug&gt;/’, methods<strong>=</strong>[“GET”]) <strong>def</strong> <strong>get_post</strong>(postid_url_slug):

<h2>GET /api/v1/p/&lt;postid&gt;/comments/</h2>

Return a list of comments for one post. Example:

$ curl -b cookies.txt “http://localhost:8000/api/v1/p/3/comments/”

{

“comments”: [

{

“commentid”: 1,

“owner”: “awdeorio”,       “owner_show_url”: “/u/awdeorio/”,

“postid”: 3,       “text”: “#chickensofinstagram”

},

{

“commentid”: 2,

“owner”: “jflinn”,       “owner_show_url”: “/u/jflinn/”,

“postid”: 3,

“text”: “I &lt;3 chickens”

},

{

“commentid”: 3,

“owner”: “michjc”,       “owner_show_url”: “/u/michjc/”,

“postid”: 3,

“text”: “Cute overload!”

}

],

“url”: “/api/v1/p/3/comments/”

}

<h2>POST /api/v1/p/&lt;postid&gt;/comments/</h2>

Add one comment to a post. Include the ID of the new comment in the return data. Return 201 on success.

HINT: sqlite3 provides a special function to retrieve the ID of the most recently inserted item:

SELECT last_insert_rowid() .

$ curl -ib cookies.txt 

–header ‘Content-Type: application/json’    –request POST 

–data ‘{“text”:”Comment sent from curl”}’   http://localhost:8000/api/v1/p/3/comments/

HTTP/1.0 201 CREATED

Content-Type: application/json

Content-Length: 135

Server: Werkzeug/0.12.2 Python/3.6.1

Date: Wed, 28 Jun 2017 17:38:06 GMT




{

“commentid”: 8,

“owner”: “awdeorio”,

“owner_show_url”: “/u/awdeorio/”,

“postid”: 3,

“text”: “Comment sent from curl”

}

The new comment appears in the list now.

$ curl -ib cookies.txt ‘http://localhost:8000/api/v1/p/3/comments/’

HTTP/1.0 200 OK

Content-Type: application/json

Content-Length: 655

Server: Werkzeug/0.12.2 Python/3.6.1

Date: Wed, 28 Jun 2017 17:38:19 GMT




{

“comments”: [

{

“commentid”: 1,

“owner”: “awdeorio”,

“owner_show_url”: “/u/awdeorio/”,

“postid”: 3,       “text”: “#chickensofinstagram”

},

{

“commentid”: 2,

“owner”: “jflinn”,       “owner_show_url”: “/u/jflinn/”,

“postid”: 3,

“text”: “I &lt;3 chickens”

},

{

“commentid”: 3,

“owner”: “michjc”,       “owner_show_url”: “/u/michjc/”,

“postid”: 3,

“text”: “Cute overload!”

},

{

“commentid”: 8,

“owner”: “awdeorio”,       “owner_show_url”: “/u/awdeorio/”,

“postid”: 3,

“text”: “Comment sent from curl”

}

],

“url”: “/api/v1/p/3/comments/”

}

<h2>GET /api/v1/p/&lt;postid&gt;/likes/</h2>

Return the number of likes for one post. Also include whether the logged in user like this ( <sub>1 </sub>) or not ( <sub>0 </sub>). Example:

$ curl -b cookies.txt “http://localhost:8000/api/v1/p/3/likes/”

{

“logname_likes_this”: 1,

“likes_count”: 1,

“postid”: 3,

“url”: “/api/v1/p/3/likes/”

}

<h2>DELETE /api/v1/p/&lt;postid&gt;/likes/</h2>

Delete one “like”. Return 204 on sucess.

$ curl -ib cookies.txt 

–header ‘Content-Type: application/json’ 

–request DELETE    –data ‘{}’ 

http://localhost:8000/api/v1/p/3/likes/

HTTP/1.0 204 NO CONTENT

Content-Type: application/json

Content-Length: 0

Server: Werkzeug/0.12.2 Python/3.6.1 Date: Wed, 28 Jun 2017 17:48:03 GMT It’s OK to delete a “like” twice.

$ curl -ib cookies.txt 

–header ‘Content-Type: application/json’ 

–request DELETE    –data ‘{}’ 

http://localhost:8000/api/v1/p/3/likes/

HTTP/1.0 204 NO CONTENT

Content-Type: application/json

Content-Length: 0

Server: Werkzeug/0.12.2 Python/3.6.1

Date: Wed, 28 Jun 2017 17:50:28 GMT




$ curl -ib cookies.txt 

–header ‘Content-Type: application/json’ 

–request DELETE    –data ‘{}’ 

http://localhost:8000/api/v1/p/3/likes/

HTTP/1.0 204 NO CONTENT

Content-Type: application/json

Content-Length: 0

Server: Werkzeug/0.12.2 Python/3.6.1

Date: Wed, 28 Jun 2017 17:50:28 GMT

<h2>POST /api/v1/p/&lt;postid&gt;/likes/</h2>

Create one “like”. Return 201 on success. Example:

$ curl -ib cookies.txt 

–header ‘Content-Type: application/json’ 

–request POST    –data ‘{}’ 

http://localhost:8000/api/v1/p/3/likes/

HTTP/1.0 201 CREATED

Content-Type: application/json

Content-Length: 44

Server: Werkzeug/0.12.2 Python/3.6.1

Date: Wed, 28 Jun 2017 17:51:30 GMT




{

“logname”: “awdeorio”,

“postid”: 3 }

If the “like” already exists, return the same data with a 409 error and JSON description.

$ curl -ib cookies.txt 

–header ‘Content-Type: application/json’ 

–request POST    –data ‘{}’ 

http://localhost:8000/api/v1/p/3/likes/

HTTP/1.0 409 CONFLICT

Content-Type: application/json

Content-Length: 44

Server: Werkzeug/0.12.2 Python/3.6.1

Date: Wed, 28 Jun 2017 17:57:59 GMT




{

“logname”: “awdeorio”,

“message”: “Conflict”,

“postid”: 3,   “status_code”: 409

}

<h2>HTTP Response codes</h2>

The Flask documentation has a helpful section on <a href="http://flask.pocoo.org/docs/0.12/patterns/apierrors/">implementing API exceptions</a><a href="http://flask.pocoo.org/docs/0.12/patterns/apierrors/">.</a> Errors returned by the REST API should take the form:

{

<strong>“message”</strong>: “&lt;describe the problem here&gt;”,

<strong>“status_code”</strong>: &lt;int goes here&gt;

…

}

All routes require a login. Return 403 if user is not logged in.

$ curl -i ‘http://localhost:8000/api/v1/’  <em># didn’t send cookies</em>

HTTP/1.0 403 FORBIDDEN

Content-Type: application/json

Content-Length: 52

Server: Werkzeug/0.12.2 Python/3.6.1

Date: Wed, 28 Jun 2017 20:12:26 GMT




{

“message”: “Forbidden”,

“status_code”: 403

}

Note that requests to user-facing pages should still return HTML. For example, if the user isn’t logged in, the / redirects to /accounts/login/ .

$ curl -i ‘http://localhost:8000/’

HTTP/1.0 302 FOUND

Content-Type: text/html; charset<strong>=</strong>utf-8

Content-Length: 239

Location: http://localhost:8000/accounts/login/

Server: Werkzeug/0.12.2 Python/3.6.1

Date: Wed, 28 Jun 2017 20:13:04 GMT




&lt;!DOCTYPE HTML PUBLIC “-//W3C//DTD HTML 3.2 Final//EN”&gt;

&lt;title&gt;Redirecting…&lt;/title&gt;

&lt;h1&gt;Redirecting…&lt;/h1&gt;

&lt;p&gt;You should be redirected automatically to target URL: &lt;a href<strong>=</strong>“/accounts/login/”<strong>&gt;</strong>/accou

Post IDs that are out of range should return a 404 error.

$ curl -b cookies.txt “http://localhost:8000/api/v1/p/1000/”

{

“message”: “Not Found”,

“status_code”: 404

}

$ curl -b cookies.txt “http://localhost:8000/api/v1/p/1000/comments/” {

“message”: “Not Found”,

“status_code”: 404

}

$ curl -b cookies.txt “http://localhost:8000/api/v1/p/1000/likes/” {

“message”: “Not Found”,

“status_code”: 404

}

<h2>Checking output style</h2>

All returned JSON should conform to standard formatting. Whitespace doesn’t matter. You can check it using <sub>jsonlint </sub>. Example:

$ curl -b cookies.txt ‘http://localhost:8000/api/v1/p/’ | jsonlint

% Total    % Received % Xferd  Average Speed   Time    Time     Time  Current

Dload  Upload   Total   Spent    Left  Speed

100   222  100   222    0     0  24960      0 –:–:– –:–:– –:–:– 27750

{

“next”: “”,

“results”: [

{

“postid”: 3,

“url”: “/api/v1/p/3/”

},

{

“postid”: 2,

“url”: “/api/v1/p/2/”

},

{

“postid”: 1,

“url”: “/api/v1/p/1/”

}

],

“url”: “/api/v1/p/” }

At this point you should be able to run the following public autograder tests on your code:

pytest -v tests/test_rest_api_query.py tests/test_rest_api_responses.py tests/test_rest_ap

…

============================================== 10 passed in 18.59 seconds ================

<h1>Client-side Insta485 Specification</h1>

This project includes the same pages as project 2. The only modified page is the index <sub>/ </sub>. All other user-facing pages are identical to project 2. This section applies only to the main page.

The main page displays the same feed of posts as in project 2. In this project, posts will be rendered client-side by JavaScript code. All posts, including comments, likes, photo, data about the user who posted, etc. must be generated from JavaScript. Continue to use server-side rendering (AKA HTML templates) for the navigation bar at the top of the page.

<strong>Pro-tip:</strong> start with a React/JS mock-up and hard coded data. Gradually add features, like retrieving data from the REST API, one at a time. See the <a href="https://facebook.github.io/react/docs/thinking-in-react.html">Thinking in React docs</a> for a good example.

<a href="https://eecs280staff.github.io/p1-stats/setup_git.html#version-control-for-a-team"><strong>Pro-tip:</strong></a><a href="https://eecs280staff.github.io/p1-stats/setup_git.html#version-control-for-a-team"> Commit and push features to your git repo, one at a time. Follow the </a><a href="https://eecs280staff.github.io/p1-stats/setup_git.html#version-control-for-a-team">Version control for a team</a><a href="https://eecs280staff.github.io/p1-stats/setup_git.html#version-control-for-a-team"> work flow.</a>

<h2>HTML Modifications from Project 2</h2>

In order for the autograder to correctly navigate the insta485 site, you’ll need to add a few HTML tags to your HTML forms. <em>This section only applies to the main page; all other pages and their HTML elements may be left as they were in project 2</em>.

The comment form must contain the id attribute “comment-form”. You may use this HTML form code. Feel free to style it and add other HTML attributes.

&lt;form id=”comment-form”&gt;

&lt;input type=”text” value=””/&gt;

&lt;/form&gt;

The like button must contain the class name attribute “like-unlike-button”. You may use this HTML code. Feel free to style it and add other HTML attributes.

&lt;button className=”like-unlike-button”&gt;

FIXME-button-text-here

&lt;/button&gt;

<h2>Main page HTML</h2>

The navigation bar should be rendered server-side, just like project 2. Include a link to <sub>/</sub> in the upper left hand corner. If not logged in, redirect to <sub>/accounts/login/ </sub>. If logged in, include a link to /explore/ and /u/&lt;user_url_slug&gt;/ in the upper right hand corner.

Here’s an outline of the rendered HTML for the main page. Notice that there is no feed content. Rather, there is an entry point for JavaScript to add the feed content.

…

&lt;body&gt;

<em>&lt;!– Plain old HTML and jinja2 nav bar goes here –&gt;</em>







&lt;div id=”reactEntry”&gt;

Loading …

&lt;/div&gt;

<em>&lt;!– Load JavaScript –&gt;</em>

&lt;script type=”text/javascript” src=”{{ url_for(‘static’, filename=’js/bundle.js’) }}”&gt;&lt;/

&lt;/body&gt;

…




<strong>Note:</strong> Rendered html text should be in the appropriate tags.

<em>// good </em>render(){

<strong>&lt;</strong>p<strong>&gt;</strong>{<strong>this</strong>.state.some_bool ? ‘Hello’ : ‘Goodbye’}<strong>&lt;</strong>/p&gt;

}

<em>// bad </em>render(){

{<strong>this</strong>.state.some_bool ? ‘Hello’ : ‘Goodbye’} }

<h2>Human readable timestamps</h2>

The API call GET /api/v1/p/&lt;postid&gt; returns the age of a post in the format Year-Month-Day Hour:Minutes:Seconds .

Your React code should convert this timestamp into human readable form e.g <sub>a few seconds </sub>ago . You should only use the <a href="https://momentjs.com/"><sub>moment.js</sub></a> library, which is already included for you in package.json, to achieve this. Using any other libraries, including react-moment, will cause you to fail tests the Autograder.

<h2>Response time</h2>

The main page should load without errors (exceptions), even when the REST API takes a long time to respond. Keep in mind that the <sub>render()</sub> method may be called asynchronously, and may be called multiple times.

Put another way, <sub>render()</sub> will likely be called by the React framework <em>before any AJAX data arrives</em>. The page should still render without errors.

<h2>Likes and comments update</h2>

Likes and comments added by the logged in user should appear immediately on the user interface without a page reload.

Comments are added by pressing the <sub>enter</sub> ( <sub>return </sub>) key. The user interface shall not contain any comment submit button. The <a href="https://facebook.github.io/react/docs/forms.html">React docs on forms</a> are very helpful for this feature.

<a href="https://eecs485staff.github.io/p3-insta485-clientside/images/demo-likes.mp4">Likes demo video.</a>

<a href="https://eecs485staff.github.io/p3-insta485-clientside/images/demo-comments.mp4">Comments update demo video.</a>

<h2>Infinite scroll</h2>

Scrolling to the bottom of the page causes additional posts to be loaded and displayed. Load and display the next 10 posts as specified by the <sub>next</sub> parameter of the most recent API call to /api/v1/p/ . Do not reload the page.

<a href="https://eecs485staff.github.io/p3-insta485-clientside/images/demo-infinitescroll.mp4">Infinite scroll demo video.</a>

We recommend the <a href="https://www.npmjs.com/package/react-infinite-scroll-component">React Infinite Scroll Component</a><a href="https://www.npmjs.com/package/react-infinite-scroll-component">,</a> which is already included in <sub>package.json </sub>.

<strong>Note</strong>: if infinite scroll has been triggered and more than 10 posts are present on the main page, reloading the page should only display the 10 most recent posts (including any new posts made before the reload).

<strong>Pro-tip</strong> to test this feature, you can use <sub>insta485db random </sub>.

(Note that in some visual demos, we use the same pictures multiple times, which might make you think that infinite scroll should at some point “cycle back to the start.” That’s NOT how it should work. Infinite scroll should keep scrolling until there are no more pictures available.)

<h2>Browser history</h2>

Don’t break the back button. Here’s an example:

<ol>

 <li>awdeorio loads <sub>/ </sub>, which displays 10 posts.</li>

 <li>awdeorio scrolls, triggering the infinite scroll mechanism. Now the page contains 20 posts.</li>

 <li>jflinn adds a new post, which updates the database.</li>

 <li>awdeorio clicks on a post to view the post details at <sub>/p/&lt;postid&gt;/ </sub>.</li>

 <li>awdeorio clicks the back button on his browser, returning to <sub>/ </sub>.</li>

 <li>The exact same 20 posts from step 2 are loaded. jflinn’s new post <em>is not included</em>.</li>

 <li>awdeorio refreshes the page. the 10 most recent posts are shown including jflinn’s new post.</li>

</ol>

As you might notice in the previous example, we do not specify whether the comments and / or likes on the 20 posts (step 6) are updated after returning to the index page using the back button. This is intentional. It is the student’s choice whether to update the comments and likes of each post after returning to the main page using the back button.

Here’s an example of two accceptable scenarios depending on the student’s choice:

Scenario 1:

<ol>

 <li>awdeorio loads <sub>/ </sub>, which displays 10 posts (post ids 1-10).</li>

 <li>awdeorio scrolls, triggering the infinite scroll mechanism. Now the page contains 20 posts (post ids 1-20).</li>

 <li>jflinn adds a new post, which updates the database.</li>

 <li>jflinn likes post id 20 (displayed to awdeorio in step 2), which updates the database.</li>

 <li>awdeorio clicks on a post to view the post details at <sub>/p/5/ </sub>.</li>

 <li>awdeorio clicks the back button on his browser, returning to <sub>/ </sub>.</li>

 <li>The exact same 20 posts from step 2 are loaded. jflinn’s new post is not included but <strong>his like on post id 20 is shown</strong>.</li>

 <li>awdeorio refreshes the page. the 10 most recent posts are shown including jflinn’s new post.</li>

</ol>

Scenario 2:

<ol>

 <li>awdeorio loads <sub>/ </sub>, which displays 10 posts (post ids 1-10).</li>

 <li>awdeorio scrolls, triggering the infinite scroll mechanism. Now the page contains 20 posts (post ids 1-20).</li>

 <li>jflinn adds a new post, which updates the database.</li>

 <li>jflinn likes post id 20 (displayed to awdeorio in step 2), which updates the database.</li>

 <li>awdeorio clicks on a post to view the post details at <sub>/p/5/ </sub>.</li>

 <li>awdeorio clicks the back button on his browser, returning to <sub>/ </sub>.</li>

 <li>The exact same 20 posts from step 2 are loaded. jflinn’s new post is not included and <strong>his like on post id 20 is not shown</strong>.</li>

 <li>awdeorio refreshes the page. the 10 most recent posts are shown including jflinn’s new post.</li>

</ol>

The same example could be given to illustrate the student’s freedom by taking scenario 1 and 2 and replacing jflinn’s “like” in step 4 with a comment instead. In this case, the student could choose whether or not to display the comment in step 7.

The Mozilla documentation on the <a href="https://developer.mozilla.org/en-US/docs/Web/API/History_API">history API</a> will be helpful.

Hint: this requires very few code modifications! Use the <a href="https://developer.mozilla.org/en-US/docs/Web/API/History_API">History API</a> to manipulate browser history and use the <a href="https://developer.mozilla.org/en-US/docs/Web/API/PerformanceNavigationTiming">PerformanceNavigationTiming API</a> to check how the user is navigating to and from a page. Don’t use other libraries for this feature (they only make it harder). Do not modify the URL.

<h2>REST API calls and logging</h2>

We’re going to grade your REST API by inspecting the server logs. We’ll also be checking that your client-side javascript is making the correct API calls by inspecting the server logs. Loading the main page with the default database configuration, while logged in as awdeorio should yield the following logs. Note that the order of likes and comments doesn’t matter.

127.0.0.1 – – [06/Jul/2017 12:02:09] “GET / HTTP/1.1” 200 –

127.0.0.1 – – [06/Jul/2017 12:02:09] “GET /static/js/bundle.js HTTP/1.1” 200 –

127.0.0.1 – – [06/Jul/2017 12:02:09] “GET /api/v1/p/ HTTP/1.1” 200 –

127.0.0.1 – – [06/Jul/2017 12:02:09] “GET /api/v1/p/3/likes/ HTTP/1.1” 200 –

127.0.0.1 – – [06/Jul/2017 12:02:09] “GET /api/v1/p/3/comments/ HTTP/1.1” 200 –

127.0.0.1 – – [06/Jul/2017 12:02:09] “GET /api/v1/p/3/ HTTP/1.1” 200 –

127.0.0.1 – – [06/Jul/2017 12:02:09] “GET /api/v1/p/2/likes/ HTTP/1.1” 200 –

127.0.0.1 – – [06/Jul/2017 12:02:09] “GET /api/v1/p/2/comments/ HTTP/1.1” 200 –

127.0.0.1 – – [06/Jul/2017 12:02:09] “GET /api/v1/p/2/ HTTP/1.1” 200 –

127.0.0.1 – – [06/Jul/2017 12:02:09] “GET /api/v1/p/1/likes/ HTTP/1.1” 200 –

127.0.0.1 – – [06/Jul/2017 12:02:09] “GET /api/v1/p/1/comments/ HTTP/1.1” 200 –

127.0.0.1 – – [06/Jul/2017 12:02:09] “GET /api/v1/p/1/ HTTP/1.1” 200 –

127.0.0.1 – – [06/Jul/2017 12:02:09] “GET /uploads/e1a7c5c32973862ee15173b0259e3efdb6a391a

127.0.0.1 – – [06/Jul/2017 12:02:09] “GET /uploads/9887e06812ef434d291e4936417d125cd594b38

127.0.0.1 – – [06/Jul/2017 12:02:09] “GET /uploads/505083b8b56c97429a728b68f31b0b2a089e511

127.0.0.1 – – [06/Jul/2017 12:02:09] “GET /uploads/ad7790405c539894d25ab8dcf0b79eed3341e10

127.0.0.1 – – [06/Jul/2017 12:02:09] “GET /uploads/122a7d27ca1d7420a1072f695d9290fad4501a4

Press the like button a couple of times:

127.0.0.1 – – [06/Jul/2017 12:03:08] “DELETE /api/v1/p/3/likes/ HTTP/1.1” 204 –

127.0.0.1 – – [06/Jul/2017 12:03:09] “POST /api/v1/p/3/likes/ HTTP/1.1” 201 – Add a comment:

127.0.0.1 – – [06/Jul/2017 12:03:27] “POST /api/v1/p/3/comments/ HTTP/1.1” 201 –

An example of infinite scroll. First, we load the main page from a database populated with 100 random posts.

127.0.0.1 – – [06/Jul/2017 12:04:59] “GET / HTTP/1.1” 200 –

127.0.0.1 – – [06/Jul/2017 12:04:59] “GET /static/js/bundle.js HTTP/1.1” 200 – 127.0.0.1 – – [06/Jul/2017 12:04:59] “GET /api/v1/p/ HTTP/1.1” 200 –

127.0.0.1 – – [06/Jul/2017 12:04:59] “GET /api/v1/p/104/likes/ HTTP/1.1” 200 –

127.0.0.1 – – [06/Jul/2017 12:04:59] “GET /api/v1/p/104/comments/ HTTP/1.1” 200 –

127.0.0.1 – – [06/Jul/2017 12:04:59] “GET /api/v1/p/104/ HTTP/1.1” 200 –

127.0.0.1 – – [06/Jul/2017 12:04:59] “GET /api/v1/p/103/likes/ HTTP/1.1” 200 –

127.0.0.1 – – [06/Jul/2017 12:04:59] “GET /api/v1/p/103/comments/ HTTP/1.1” 200 –

127.0.0.1 – – [06/Jul/2017 12:04:59] “GET /api/v1/p/103/ HTTP/1.1” 200 –

127.0.0.1 – – [06/Jul/2017 12:04:59] “GET /api/v1/p/101/likes/ HTTP/1.1” 200 –

127.0.0.1 – – [06/Jul/2017 12:04:59] “GET /api/v1/p/101/comments/ HTTP/1.1” 200 –

127.0.0.1 – – [06/Jul/2017 12:04:59] “GET /api/v1/p/101/ HTTP/1.1” 200 –

127.0.0.1 – – [06/Jul/2017 12:04:59] “GET /api/v1/p/100/likes/ HTTP/1.1” 200 –

127.0.0.1 – – [06/Jul/2017 12:04:59] “GET /api/v1/p/100/comments/ HTTP/1.1” 200 –

127.0.0.1 – – [06/Jul/2017 12:04:59] “GET /api/v1/p/100/ HTTP/1.1” 200 –

127.0.0.1 – – [06/Jul/2017 12:04:59] “GET /api/v1/p/99/likes/ HTTP/1.1” 200 –

127.0.0.1 – – [06/Jul/2017 12:04:59] “GET /api/v1/p/99/comments/ HTTP/1.1” 200 –

127.0.0.1 – – [06/Jul/2017 12:04:59] “GET /api/v1/p/99/ HTTP/1.1” 200 –

127.0.0.1 – – [06/Jul/2017 12:04:59] “GET /api/v1/p/98/likes/ HTTP/1.1” 200 –

127.0.0.1 – – [06/Jul/2017 12:04:59] “GET /api/v1/p/98/comments/ HTTP/1.1” 200 –

127.0.0.1 – – [06/Jul/2017 12:04:59] “GET /api/v1/p/98/ HTTP/1.1” 200 –

127.0.0.1 – – [06/Jul/2017 12:04:59] “GET /api/v1/p/96/likes/ HTTP/1.1” 200 –

127.0.0.1 – – [06/Jul/2017 12:04:59] “GET /api/v1/p/96/comments/ HTTP/1.1” 200 –

127.0.0.1 – – [06/Jul/2017 12:04:59] “GET /api/v1/p/96/ HTTP/1.1” 200 –

127.0.0.1 – – [06/Jul/2017 12:04:59] “GET /api/v1/p/95/likes/ HTTP/1.1” 200 –

127.0.0.1 – – [06/Jul/2017 12:04:59] “GET /api/v1/p/95/comments/ HTTP/1.1” 200 –

127.0.0.1 – – [06/Jul/2017 12:04:59] “GET /api/v1/p/95/ HTTP/1.1” 200 –

127.0.0.1 – – [06/Jul/2017 12:04:59] “GET /api/v1/p/94/likes/ HTTP/1.1” 200 –

127.0.0.1 – – [06/Jul/2017 12:04:59] “GET /api/v1/p/94/comments/ HTTP/1.1” 200 –

127.0.0.1 – – [06/Jul/2017 12:04:59] “GET /api/v1/p/94/ HTTP/1.1” 200 –

127.0.0.1 – – [06/Jul/2017 12:04:59] “GET /api/v1/p/93/likes/ HTTP/1.1” 200 –

127.0.0.1 – – [06/Jul/2017 12:04:59] “GET /api/v1/p/93/comments/ HTTP/1.1” 200 –

127.0.0.1 – – [06/Jul/2017 12:04:59] “GET /api/v1/p/93/ HTTP/1.1” 200 –

127.0.0.1 – – [06/Jul/2017 12:04:59] “GET /uploads/505083b8b56c97429a728b68f31b0b2a089e511

127.0.0.1 – – [06/Jul/2017 12:04:59] “GET /uploads/e1a7c5c32973862ee15173b0259e3efdb6a391a

127.0.0.1 – – [06/Jul/2017 12:04:59] “GET /uploads/ad7790405c539894d25ab8dcf0b79eed3341e10

127.0.0.1 – – [06/Jul/2017 12:04:59] “GET /uploads/5ecde7677b83304132cb2871516ea50032ff7a4

127.0.0.1 – – [06/Jul/2017 12:04:59] “GET /uploads/122a7d27ca1d7420a1072f695d9290fad4501a4

127.0.0.1 – – [06/Jul/2017 12:04:59] “GET /uploads/2ec7cf8ae158b3b1f40065abfb33e8114370784

127.0.0.1 – – [06/Jul/2017 12:04:59] “GET /uploads/9887e06812ef434d291e4936417d125cd594b38

Scroll to the bottom and infinite scroll is triggered.

127.0.0.1 – – [06/Jul/2017 12:05:18] “GET /api/v1/p/?size=10&amp;page=1 HTTP/1.1” 200 –

127.0.0.1 – – [06/Jul/2017 12:05:18] “GET /api/v1/p/91/likes/ HTTP/1.1” 200 –

127.0.0.1 – – [06/Jul/2017 12:05:18] “GET /api/v1/p/91/comments/ HTTP/1.1” 200 –

127.0.0.1 – – [06/Jul/2017 12:05:18] “GET /api/v1/p/91/ HTTP/1.1” 200 –

127.0.0.1 – – [06/Jul/2017 12:05:18] “GET /api/v1/p/90/likes/ HTTP/1.1” 200 –

127.0.0.1 – – [06/Jul/2017 12:05:18] “GET /api/v1/p/90/comments/ HTTP/1.1” 200 –

127.0.0.1 – – [06/Jul/2017 12:05:18] “GET /api/v1/p/90/ HTTP/1.1” 200 –

127.0.0.1 – – [06/Jul/2017 12:05:18] “GET /api/v1/p/89/likes/ HTTP/1.1” 200 –

127.0.0.1 – – [06/Jul/2017 12:05:18] “GET /api/v1/p/89/comments/ HTTP/1.1” 200 –

127.0.0.1 – – [06/Jul/2017 12:05:18] “GET /api/v1/p/89/ HTTP/1.1” 200 –

127.0.0.1 – – [06/Jul/2017 12:05:18] “GET /api/v1/p/87/likes/ HTTP/1.1” 200 –

127.0.0.1 – – [06/Jul/2017 12:05:18] “GET /api/v1/p/87/comments/ HTTP/1.1” 200 –

127.0.0.1 – – [06/Jul/2017 12:05:18] “GET /api/v1/p/87/ HTTP/1.1” 200 –

127.0.0.1 – – [06/Jul/2017 12:05:18] “GET /api/v1/p/85/likes/ HTTP/1.1” 200 –

127.0.0.1 – – [06/Jul/2017 12:05:18] “GET /api/v1/p/85/comments/ HTTP/1.1” 200 –

127.0.0.1 – – [06/Jul/2017 12:05:18] “GET /api/v1/p/85/ HTTP/1.1” 200 –

127.0.0.1 – – [06/Jul/2017 12:05:18] “GET /api/v1/p/84/likes/ HTTP/1.1” 200 –

127.0.0.1 – – [06/Jul/2017 12:05:18] “GET /api/v1/p/84/comments/ HTTP/1.1” 200 –

127.0.0.1 – – [06/Jul/2017 12:05:18] “GET /api/v1/p/84/ HTTP/1.1” 200 –

127.0.0.1 – – [06/Jul/2017 12:05:18] “GET /api/v1/p/83/likes/ HTTP/1.1” 200 –

127.0.0.1 – – [06/Jul/2017 12:05:18] “GET /api/v1/p/83/comments/ HTTP/1.1” 200 –

127.0.0.1 – – [06/Jul/2017 12:05:18] “GET /api/v1/p/83/ HTTP/1.1” 200 –

127.0.0.1 – – [06/Jul/2017 12:05:18] “GET /api/v1/p/81/likes/ HTTP/1.1” 200 –

127.0.0.1 – – [06/Jul/2017 12:05:18] “GET /api/v1/p/81/comments/ HTTP/1.1” 200 –

127.0.0.1 – – [06/Jul/2017 12:05:18] “GET /api/v1/p/81/ HTTP/1.1” 200 –

127.0.0.1 – – [06/Jul/2017 12:05:18] “GET /api/v1/p/80/likes/ HTTP/1.1” 200 –

127.0.0.1 – – [06/Jul/2017 12:05:18] “GET /api/v1/p/80/comments/ HTTP/1.1” 200 –

127.0.0.1 – – [06/Jul/2017 12:05:18] “GET /api/v1/p/80/ HTTP/1.1” 200 –

127.0.0.1 – – [06/Jul/2017 12:05:18] “GET /api/v1/p/79/likes/ HTTP/1.1” 200 –

127.0.0.1 – – [06/Jul/2017 12:05:18] “GET /api/v1/p/79/comments/ HTTP/1.1” 200 – 127.0.0.1 – – [06/Jul/2017 12:05:18] “GET /api/v1/p/79/ HTTP/1.1” 200 –

<h2>Code style</h2>

As in project 2, all HTML should be W3C compliant, as reported by <sub>html5validator </sub>. Python code should be contain no errors or warnings from <sub>pycodestyle </sub>, <sub>pydocstyle </sub>, and <sub>pylint </sub>. Use pylint –disable=cyclic-import .

All JSON returned by the REST API should be <sub>jsonlint</sub> clean.

All JavaScript source code should conform to the AirBnB javascript coding standard. Use <sub>eslint </sub>to test it. Refer to the <a href="https://eecs485staff.github.io/p3-insta485-clientside/setup_react.html#eslint">setup / eslint</a> tutorial.

You may only use JavaScript libraries that are contained in <sub>package.json</sub> from the starter files and the built-in <a href="https://developer.mozilla.org/en-US/docs/Web/API">Web APIs</a>.

You must use the <sub>fetch</sub> API for AJAX calls.

<strong>Can I disable any code style checks?</strong>

Do not disable any code style check from any python code style tool ( <sub>pycodestyle </sub>, <sub>pydocstyle </sub>, pylint ), besides the three exceptions specified in the <a href="https://eecs485staff.github.io/p2-insta485-serverside/#can-i-disable-any-code-style-checks">Project 2 spec</a>

Additionally, do not disable any <sub>eslint</sub> checks in your <sub>.jsx</sub> files. There are <strong>no exceptions</strong> to this rule.

<h2>Testing</h2>

Make sure that you’ve completed the <a href="https://eecs485staff.github.io/p3-insta485-clientside/setup.html#end-to-end-testing">End-to-end testing tutorial</a><a href="https://eecs485staff.github.io/p3-insta485-clientside/setup.html#end-to-end-testing">.</a>

Several unit tests are published with the starter files. Make sure you’ve copied the <sub>tests </sub>directory. Note that your files may be slightly different.

$ pwd

/Users/awdeorio/src/eecs485/p3-insta485-clientside

$ ls tests/

73ab33bd357c3fd42292487b825880958c595655.jpg  test_base_rest_api.py config.py                                     test_index.py insta485_logging.py                           test_rest_api_responses.py insta485_logging_slow.py                      test_slow_server_index.py test_base_live.py                             util.py

Rebuild your javascript bundles by running webpack and then run the tests.

$ pwd

/Users/awdeorio/src/eecs485/p3-insta485-clientside

$ ./node_modules/.bin/webpack

Hash: 94d02a55e475959ef08d

Version: webpack 2.6.1

Time: 4910ms

[…. Truncated output …]

$ pytest -v

<a href="https://eecs485staff.github.io/p1-insta485-static/setup_pytest.html#deprecation-warnings">Note: if you get deprecation warnings from third party libraries, check out the </a><a href="https://eecs485staff.github.io/p1-insta485-static/setup_pytest.html#deprecation-warnings">pytest tutorial deprecation warnings</a><a href="https://eecs485staff.github.io/p1-insta485-static/setup_pytest.html#deprecation-warnings"> to suppress them.</a>

<strong>insta485test</strong>

Add JavaScript style checking you <sub>insta485test</sub> script from project 2. In addition to the tests run in project 2, <sub>insta485test</sub> should run <sub>eslint</sub> on all files within the <sub>insta485/js/</sub> directory. Refer back to the <a href="https://eecs485staff.github.io/p3-insta485-clientside/setup_react.html#eslint"><sub>eslint</sub></a><a href="https://eecs485staff.github.io/p3-insta485-clientside/setup_react.html#eslint"> instructions</a> for direction on how to do this.

$ ./bin/insta485test

<h2>Deploy to AWS</h2>

<a href="https://eecs485staff.github.io/p2-insta485-serverside/setup_aws.html#deploy-a-web-app">You should have already created an AWS account and instance (</a><a href="https://eecs485staff.github.io/p2-insta485-serverside/setup_aws.html#deploy-a-web-app">instructions</a><a href="https://eecs485staff.github.io/p2-insta485-serverside/setup_aws.html#deploy-a-web-app">). </a><a href="https://eecs485staff.github.io/p2-insta485-serverside/setup_aws.html#deploy-a-web-app">Resume the </a><a href="https://eecs485staff.github.io/p2-insta485-serverside/setup_aws.html#deploy-a-web-app">Project 2 AWS Tutorial – Deploy a web app </a><a href="https://eecs485staff.github.io/p2-insta485-serverside/setup_aws.html#deploy-a-web-app">.</a>

After you have deployed your site, download the main page along with a log. Do this from your local machine.

$ pwd

/Users/awdeorio/src/eecs485/p3-insta485-serverside

$ curl 

–request POST 

–cookie-jar cookies.txt 

–form ‘username=awdeorio’ 

–form ‘password=password’ 

–form ‘submit=login’ 

“http://&lt;Public DNS (IPv4)&gt;/accounts/login/”

$ curl -v -b cookies.txt “http://&lt;Public DNS (IPv4)&gt;/” <strong>&gt;</strong> deployed_index.html 2&gt; deployed_i

$ curl -v -b cookies.txt “http://&lt;Public DNS (IPv4)&gt;/static/js/bundle.js” <strong>&gt;</strong> deployed_bundl

Be sure to verify that the output in deployed_index.log and deployed_bundle_js.log doesn’t include errors like “Couldn’t connect to server”. If it does contain an error like this, it means <sub>curl </sub>couldn’t successfully connect with your flask app.

Also be sure to verify that the output in <sub>deployed_index.html</sub> looks like the <sub>index.html</sub> file you coded while <sub>deployed_bundle_js.js</sub> contains Javascript code.

<h2>Submitting and grading</h2>

One team member should register your group on the autograder using the <em>create new invitation </em>feature.

Submit a tarball to the autograder, which is linked from <a href="https://eecs485.org/">https://eecs485.org</a><a href="https://eecs485.org/">.</a> Include the <sub>–</sub>disable-copyfile flag only on macOS.

$ tar 

–disable-copyfile 

–exclude ‘*__pycache__*’    -czvf submit.tar.gz    bin    insta485    package-lock.json    package.json    setup.py sql    webpack.config.js    deployed_index.html    deployed_index.log    deployed_bundle_js.js    deployed_bundle_js.log

The autograder will run pip install -e YOUR_SOLUTION and cd YOUR_SOLUTION &amp;&amp; npm install . . The exact library versions in starter_files/setup.py and package.json are cached on the autograder, so be sure not to add extra library dependencies to either one.




<h2>FAQ</h2>

<strong>My JavaScript code doesn’t work. What do I do?</strong>

<ol>

 <li>Make sure it’s <sub>eslint</sub> <a href="https://eecs485staff.github.io/p3-insta485-clientside/setup_react.html#eslint">Instructions here</a>.</li>

 <li>Make sure it’s free from exceptions by checking the <a href="https://developers.google.com/web/tools/chrome-devtools/console/">developer console</a> for exception messages</li>

 <li>Try the <a href="https://github.com/facebook/react-devtools">React Developer tools</a> Chrome extension</li>

 <li><a href="https://busypeoples.github.io/post/react-component-lifecycle/">Check your assumptions about when React methods are called. This is called the </a><a href="https://busypeoples.github.io/post/react-component-lifecycle/">React component lifecycle</a><a href="https://busypeoples.github.io/post/react-component-lifecycle/">. </a><a href="https://busypeoples.github.io/post/react-component-lifecycle/">Add </a><a href="https://busypeoples.github.io/post/react-component-lifecycle/"><sub>log()</sub></a><a href="https://busypeoples.github.io/post/react-component-lifecycle/"> messages to each React method (</a> <a href="https://busypeoples.github.io/post/react-component-lifecycle/"><sub>constructo</sub></a><sub>r() </sub>, render() , etc.).</li>

</ol>

<strong>How do I use the </strong><strong>fetch</strong><strong> API?</strong>

Refer to the <a href="https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch">Fetch API documentation</a> for a brief tutorial on how to use fetch.

Don’t forget <sub>credentials: ‘include’ </sub>! Make sure you aren’t using <sub>host:port</sub> in your client fetch URL.

<strong>Can I use </strong><strong>jQuery </strong><strong>?</strong>

Do not use <sub>jQuery</sub> for this project. Doing so is likely to cause issues with the Autograder.

The only external libraries that are needed for this project are already included for you in starter_files/package.json .

<strong>Can I use </strong><strong>XMLHttpRequest </strong><strong>?</strong>

Do not use <sub>XMLHttpRequest</sub> to make HTTP requests. Instead use the <sub>fetch</sub> API detailed above <strong>How do I make a button toggle?</strong>

Toggle buttons are useful for the like/like button. See this <a href="https://facebook.github.io/react/docs/handling-events.html">example</a> for more information.

<strong>Do trailing slashes in URLs matter to Flask?</strong>

Yes. Use them with the <sub>route</sub> decorator your REST API. See the “Unique URLs / Redirection Behavior” section in the <a href="http://flask.pocoo.org/docs/0.12/quickstart/">Flask quickstart</a><a href="http://flask.pocoo.org/docs/0.12/quickstart/">.</a> Here’s a good example:

<strong>@</strong>insta485<strong>.</strong>app<strong>.</strong>route(“/u/&lt;username_url_slug&gt;/”, methods<strong>=</strong>[“GET”, “POST”])

<strong>Can we use </strong><strong>console.log() </strong><strong>?</strong>

Yes. Ideally you should only log in the case of an error.

<h3>eslint Error … “is missing in props validation”</h3>

You’ll probably encounter this error while running <sub>eslint </sub>:

$ eslint –ext jsx insta485/js/

24:38  error    ‘url’ is missing in props validation  react/prop-types

With <sub>prop-types </sub>, you’ll get a nice error in the console when a type property is violated at run time. For example,

“Warning: Failed propType: The prop <sub>url</sub> is marked as required in <sub>CommentInput </sub>, but its value is <sub>undefined </sub>. Check the render method of <sub>Comments </sub>.

More on the <sub>prop-types</sub> library: <a href="https://www.npmjs.com/package/prop-types">https://www.npmjs.com/package/prop-types</a><a href="https://www.npmjs.com/package/prop-types">.</a>

<strong>How do I append to an array of mutable state in a React Component?</strong>

Be really careful when both reading and writing <sub>this.state.MY_VARIABLE </sub>. Here’s how.

<strong>this</strong>.setState(prevState <strong>=&gt;</strong> ({

posts: prevState.posts.concat(data.results), }));

<h3>Reconciliation and keys</h3>

When using a collection of React components, they need to have unique <sub>key</sub> properties. This enables the fast shadow DOM to real DOM update performed by react. More info here:

<a href="https://facebook.github.io/react/docs/reconciliation.html#keys">https://facebook.github.io/react/docs/reconciliation.html#keys </a><a href="https://facebook.github.io/react/docs/lists-and-keys.html#keys">https://facebook.github.io/react/docs/lists-and-keys.html#keys</a>

<h3>How to fix pylint “Similar lines in 2 files”</h3>

The REST API shares some code in common with portions of insta485’s static pages that haven’t been modified. For example, both the REST API and the static <sub>/u/&lt;postid&gt;/</sub> read the comments and likes from the database. This could lead to <sub>pylint</sub> detecting copy paste errors.

************* Module insta485.views.user

R:  1, 0: Similar lines in 2 files

==insta485.api.comments:30

==insta485.views.post:42

A nice way to resolve this problem is by adding helper functions to your <sub>model </sub>. The canonical way to solve this problem is an Object Relational Model (ORM), but we’re simplifying in this project.


