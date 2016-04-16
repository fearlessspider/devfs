# Django
## Serving multiple Django applications with Nginx and Gunicorn

Enable the virtual servers and restart Nginx:

```
$ sudo ln -s /etc/nginx/sites-available/hello /etc/nginx/sites-enabled/hello
$ sudo ln -s /etc/nginx/sites-available/goodbye /etc/nginx/sites-enabled/goodbye
$ sudo service nginx restart
```