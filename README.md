# These are the steps to deploy react app with nginx
## If you don't have nodejs
To get this version, you can use the apt package manager. Refresh your local package index by typing:
```
sudo apt update
```
install nodejs and npm if you dont have them in the server
```
sudo apt install nodejs
sudo apt install npm
```

To check which version of Node.js 
```
nodejs -v
```
To check which version of npm
```
npm -v
```

## step #1

install nginx
```
sudo apt-get install nginx
```

## step #2 
Clone your repository on your machine
you can eiter clone or copy you files
```
sudo mkdir /var/www/[your project name]
cd /var/www/[your project name]
```
## step #3
run the following commands to build your react app
```
npm i
npm i serve -g
npm run build
serve -s build
```
#### OR 
build it on your machine and run
```
npm i
npm i serve -g
serve -s build
```
## step #4 you can skip this part if you are root
In your project directery
Since you wouldn’t want to use sudo every time you interact with files in /var/www/[your project name], let’s give your user privileges to these folders. 
Constantly using sudo increases the chances of accidentally trashing your system.
```
sudo gpasswd -a "$USER" www-data
sudo chown -R "$USER":www-data /var/www/[your project name]
find /var/www/[your project name] -type f -exec chmod 0660 {} \;
sudo find /var/www/[your project name] -type d -exec chmod 2770 {} \;
```
## step #5
Create a new site in sites available
this command will create a file that you can edit
```
cd /etc/nginx/sites-available
nano [name of your website].com
```
Configure new site
Open krim.com in an editor of your choice and paste the following. 
You can safely use an ip address here.
You can also use both ip and domain.
you can copy the folowing code and change things in the bracket
##### For test deployment Without https
```
server {
  listen [port]; ## put your port number example 80
  server_name [website url]; ## put your website url example oman.com remove this line if you dont have url
  server_name [server IP]; ## put your server IP example 111.111.111.111
  root /var/www/[your project name]/build;
  index index.html index.htm;
  client_max_body_size 100M; ## size of files can be sent in the body

  access_log /var/log/nginx/[name of your website].com.access.log;
  error_log /var/log/nginx/[name of your website].com.error.log;
  location / {
    try_files $uri /index.html =404;
  }
}
```
##### With https
```
server {
  listen 443 ssl;
  listen [::]:443 ssl;
  server_name [website url]; ## put your website url example oman.com remove this line if you dont have url
  server_name [server IP]; ## put your server IP example 111.111.111.111
  root /var/www/[your project name]/build;
  index index.html index.htm;
  client_max_body_size 100M; ## size of files can be sent in the body

  access_log /var/log/nginx/[name of your website].com.access.log;
  error_log /var/log/nginx/[name of your website].com.error.log;
  location / {
    try_files $uri /index.html =404;
  }
}
```

## step #6
Symlink config
Nginx needs to be told that a given site is active. To do this, create a symlink in sites-enabled
```
cd /etc/nginx/sites-enabled
ln -s ../sites-available/[name of your website].com .
```
## step #7

Now start up Nginx!
```
sudo service nginx start
```
If you changed up your repository or made any changes to the configuration, you can restart Nginx with:
```
sudo service nginx restart
```
And you’re done! If you go to your browser and type in the IP address of your server or your domain, you should see your React app live!

# sources
* https://medium.com/@timmykko/deploying-create-react-app-with-nginx-and-ubuntu-e6fe83c5e9e7
* https://codeburst.io/how-to-setup-nginx-for-react-a504f38f95ed

