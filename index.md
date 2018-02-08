## Welcome to GitHub Pages

## The fullstack developer handy guide to quick deployment
A simple blog to note down the hard learned lessons to fullstack development.

## Setting up production ready nginx server
- Use phusion passenger + nginx + nodejs(or others)

## Nginx configurations
edit /etc/nginx/nginx.conf, change the lines that read "include sites-enabled/*;" or "include /etc/nginx/sites-enabled/*;" to read 
"include /etc/nginx/sites-available/*;", to avoid needing to symlink every file from /sites-available to /sites-enabled,
which is confusing.

## /etc/nginx/sites-available/xxx.conf
A server block typically looks like this:
`server
{
    listen 80;
    listen [::]:80;
    server_name domain01.com www.domain01.com;

    location /
    {
        proxy_pass http://127.0.0.1:3626;
        include /etc/nginx/proxy_params;
    }
}`

the value of server_name is crucial, it must be set to the real web address of the server or else it may not work properly.
eg. server_name www.mydomain.com


You can use the [editor on GitHub](https://github.com/kelvinAI/fullstack-blog/edit/master/index.md) to maintain and preview the content for your website in Markdown files.

Whenever you commit to this repository, GitHub Pages will run [Jekyll](https://jekyllrb.com/) to rebuild the pages in your site, from the content in your Markdown files.

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
