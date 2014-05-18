<!--
PostId: 964044214081748856
Title    : Nginx reverse proxy with SSL offloading and authentication
Labels   : nginx linux
Format   : markdown
-->

### What's it

Recently we've started down the ElasticSearch/Kibana road and also had a handful of other node.js apps. Setting up
access control for these was becoming a pain - especially with ES which doesn't come with any authentication/security
out of the box. General advice on the interwebs is to let upstream server handle authentication and SSL + basic auth
is good enough.

### Install nginx

1. `sudo apt-get install nginx apache2-utils`
    Apache2 utils is for generating the htpasswd file for use later.

### Configuring nginx

There's a few things to handle:

1. we want SSL offloading to happen
2. Each application is going to be set up as it's own virtual host - so think app1.mydomain.com, app2.mydomain.com
3. All apps will be served on SSL. HTTP requests will be forwarded to HTTPS automatically.

<script src="https://gist.github.com/raghur/235d6b94436ffd6f3ef1.js"></script>

### Password db

You can create the password db with
```
htpasswd -c /etc/nginx/passworddb <username>
```

You'll be asked to confirm a password for the user and you're all set.


### Reverse proxy server variables

See the last one setting a few other server variables - X-Forwarded-For, X-Forwarded-Host, X-Forwarded-Proto? You'll
need this if the app you're serving is written to be reverse proxy aware - in that case, it would use these headers to
generate any links in the html it generates (so that it doesn't return a document with `http://internalhost:port/path` - and
instead returns `http://reverseproxyhost/path`). If the app you're proxying doesn't do this, then  you're in the realm
of output content rewriting (basically regex the output content and replace link - ughhh!)
