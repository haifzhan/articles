# How to Deploy Angular Project on GoDaddy?
## Step 0:
Create an Angular project with name `my-project` and compile the app by `ng build --prod`, 
then the output directory would be `/path/to/my-project/dist/my-project` and you can see the compiled js files and `assets` folder.

## Step 1:
Opn FileZilla, connect to GoDaddy via FTP, upload folder `my-project` (this is the sub-folder which contains all compiled files) to GoDaddy

## Step 2:
Go to `cPanel Admin`, click `File Manager` and find `my-project` based on your account, then move all contents under `my-project` to `public_html`, then GoDaddy will use `index.html` automatically.

## Important 
If your application contains multiple routes, you probably get route error 404, the solution is create `.htaccess` under folder `public_html`:
```
<IfModule mod_rewrite.c>
  Options Indexes FollowSymLinks
  RewriteEngine On
  RewriteBase /myappdirectory/
  RewriteRule ^index\.html$ - [L]
  RewriteCond %{REQUEST_FILENAME} !-f
  RewriteCond %{REQUEST_FILENAME} !-d
  RewriteRule . /index.html [L]
</IfModule>
``` 

Hope it helps, happy coding!