## Welcome to GitHub Pages

## The fullstack developer handy guide to quick deployment
A simple blog to note down the hard learned lessons to fullstack development.

## Setting up production ready nginx server
- Use phusion passenger + nginx + nodejs(or others)

## Nginx configurations

Place all the available configuration files in 
```
include /etc/nginx/sites-available/*;

``` 

Enable only those files that are needed by creating a symbolic link.

First, cd into the ```/etc/nginx/sites-enabled/``` folder

Then run
```
sudo ln -s /etc/nginx/sites-available/xxx.config .
```
to create the symbolic link in the sites-enabled folder to 'activate' that particular configuration. This is a good way manage multiple configurations 



## /etc/nginx/sites-available/xxx.conf
A server block typically looks like this:
```
server
{
    listen 80;
    listen [::]:80;
    server_name domain01.com www.domain01.com;

    location /
    {
        proxy_pass http://127.0.0.1:3626;
        include /etc/nginx/proxy_params;
    }
}

```

## AWS ELB SSL
To enable SSL termination on ELB, do :
Request certificates for *.example.com , www.example.com (should have the same cert with *.example.com) and example.com   
In Amazon route 53, add ALIAS for BOTH example.com, AND *.example.com and point to the load balancer

the value of server_name is crucial, it must be set to the real web address of the server or else it may not work properly.
eg. server_name www.mydomain.com

check out https://www.digitalocean.com/community/questions/setting-up-multiple-nodejs-applications-using-nginx-vitual-hosts for more infomation.


You can use the [editor on GitHub](https://github.com/kelvinAI/fullstack-blog/edit/master/index.md) to maintain and preview the content for your website in Markdown files.

Whenever you commit to this repository, GitHub Pages will run [Jekyll](https://jekyllrb.com/) to rebuild the pages in your site, from the content in your Markdown files.


## Serving static pages (built with npm run build, etc.. )
nginx basic configurations:
```
server {
  listen 80;
  server_name app.mydomain.com;
  root /srv/app-name;
  index index.html;
  # Other config you desire (logging, etc)...
  location / {
    try_files $uri /index.html;
  }
}

```
Important note!! must use 
```
try_files $uri /index.html
```
or else page might fail during redirecting / new tabs!!


### Troubeshooting blank page in create-react app served in subfolder instead of root 
If you are trying to serve a nodejs app made with create-react in a subfolder , ie. http://mysite.com/version2/, you need to change the baseurl in the create-react router otherwise it will not route correctly and the page will be blank.


## Setting up a flask app on NGINX, phusion passenger, Anaconda (for easier python env management)

1. Set up nginx config, preferably use a different subdomain, eg. analytics.website.com/ or test.website.com/ as there may be
problems serving different applications within the same domain, as the NGINX root and ALIAS path gets complicated and prone
to errors

2. Install Anaconda. Use the 64 bit python3 version. Remember to run the installer as the application user e.g myappuser or www, not the root user that configures the system.
```
wget https://repo.continuum.io/archive/Anaconda3-5.1.0-Linux-x86_64.sh
bash ~/Downloads/Anaconda3-5.1.0-Linux-x86_64.sh`
```
Now create environment for the flask app
```
conda create -n flask-env 
```
If there are multiple permission errors, run chown to fix them one by one. Run as system user
```
sudo chown -R  appuser:appuser /home/appuser/.conda/environments.txt
sudo chown -R appuser:appuser /home/appuser/.conda/

```

Install necessary packages, activate environment first
```
source activate flask-env
conda install flask pandas numpy

```

git pull or create the flask application in the web folder

Ensure that there's a passenger_wsgi.py in the root folder of the flask app. It'll look something like this

```
import sys,os
INTERP = '/home/myappuser/anaconda3/envs/flaskenv/bin/python'
if sys.executable != INTERP:
        os.execl(INTERP, INTERP, *sys.argv)
sys.path.append('/home/myappuser/anaconda3/envs/flaskenv/lib/python3.6/site-packages')

from app import MyApp as application

```

The `sys.path.append('/home/myappuser/anaconda3/envs/flaskenv/lib/python3.6/site-packages')` is important to allow the discovery of 
installed modules in the python environment.
Point INTERP to the  correct python executable that you've just created earlier

Now go back to editing nginx configuration file in /etc/nginx/sites-available/xxx.config

After that, cd into sites-enabled directory and create a symbolic link there to enable the new server block
`sudo ln -s /etc/nginx/sites-available/xxx.config`

### Markdown

Markdown is a lightweight and easy-to-use syntax for styling your writing. It includes conventions for

```markdown
Syntax highlighted code block

# Header 1
## Header 2
### Header 3

- Bulleted
- List

1. Numbered
2. List

**Bold** and _Italic_ and `Code` text

[Link](url) and ![Image](src)
```

For more details see [GitHub Flavored Markdown](https://guides.github.com/features/mastering-markdown/).

### Jekyll Themes

Your Pages site will use the layout and styles from the Jekyll theme you have selected in your [repository settings](https://github.com/kelvinAI/fullstack-blog/settings). The name of this theme is saved in the Jekyll `_config.yml` configuration file.

### Support or Contact

Having trouble with Pages? Check out our [documentation](https://help.github.com/categories/github-pages-basics/) or [contact support](https://github.com/contact) and we’ll help you sort it out.
