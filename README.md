# sheepy-hosting

I'll eventually move this to somewhere more permanent but for now I'm brain dumping here. 

[toc]

## Connecting via SSH
Every customer has their own VM.

As currently there is only a single IP then each customer will be given the following:

* A user name
* A port to connect to

For example lets assume the following:

* **User**: john
* **Port**: 15123

From the command line you can do:
```
ssh -p 15123 john@phy1.farm.sheepy.org
```

Or you can add the following to ```~/.ssh/config``` on Linux or OS X
```
Hostname phy1.farm.sheepy.org
  User john
  Port 15123
```
then you can just do
```
ssh phy1.farm.sheepy.org
```

If you want something easier to remember you could have:
```
Hostname sheepy
  Host phy1.farm.sheepy.org
  User john
  Port 15123
```
then you can just do
```
ssh sheepy
```

## PHP Hosting

The setup differs somewhat to what you have been used to before.
We are now running nginx as the web server and PHP is running under php-fpm.

The main effect of this is that .htaccess do not work any more so for the moment I'll have to add any redirects etc...

### Where stuff lives

Your site directory is located in ```~/www/<domain>/```

### Permissions

Since each user now has their own VM you can be a bit more relaxed about file permissions.
Remember that there are 2 processes going on here:

* php-fpm: which runs as your user. So .php files need to be at least readable by you.
* nginx: which runs as the www-data user. So all static files should be globally readable.

### MySQL

If you require a database then you will be given the credentials. This lives on a separate host to your code.

### Load Balancer

Everything now lives behind a shared load balancer. The main effect of this is that if you're looking at IP
addresses for any reason (GeoIP or similar) you'll need to use the ```X-Forwarded-For header```


