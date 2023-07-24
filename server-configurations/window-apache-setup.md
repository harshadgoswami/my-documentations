To set up Apache with two virtual hosts on a Windows machine, one for a domain and another for a subdomain, follow these steps:

Step 1: Install Apache HTTP Server

1. Download the Apache HTTP Server from the Apache Lounge website: https://www.apachelounge.com/download/
2. Choose the appropriate version of Apache for your Windows architecture (32-bit or 64-bit).
3. Extract the downloaded file to a directory on your computer (e.g., `C:\Apache24`).

Step 2: Configure Apache Virtual Hosts

1. Open the Apache configuration file `httpd.conf`. You can find it in the `conf` directory of your Apache installation.
2. Uncomment the line `Include conf/extra/httpd-vhosts.conf` by removing the `#` at the beginning of the line.
3. Save the changes and close the file.

Step 3: Configure Virtual Hosts

1. Open the Apache virtual hosts configuration file `httpd-vhosts.conf`. You can find it in the `conf/extra` directory of your Apache installation.
2. Add the following configuration for the domain virtual host:

```apache
<VirtualHost *:80>
    ServerName yourdomain.com
    DocumentRoot "C:/Apache24/htdocs/yourdomain"
</VirtualHost>
```

Replace `yourdomain.com` with your actual domain name and `C:/Apache24/htdocs/yourdomain` with the path to the directory where your domain files reside.

3. Add the following configuration for the subdomain virtual host:

```apache
<VirtualHost *:80>
    ServerName subdomain.yourdomain.com
    DocumentRoot "C:/Apache24/htdocs/subdomain"
</VirtualHost>
```

Replace `subdomain.yourdomain.com` with your actual subdomain and `C:/Apache24/htdocs/subdomain` with the path to the directory where your subdomain files reside.

4. Save the changes and close the file.

Step 4: Modify hosts file

1. Open the hosts file located at `C:\Windows\System32\drivers\etc\hosts` using a text editor with administrator privileges.
2. Add the following entries at the end of the file:

```
127.0.0.1 yourdomain.com
127.0.0.1 subdomain.yourdomain.com
```

Replace `yourdomain.com` and `subdomain.yourdomain.com` with your actual domain and subdomain names. 3. Save the changes and close the file.

Step 5: Test the configuration

1. Start the Apache server by running `httpd.exe` located in the `bin` directory of your Apache installation.
2. Open a web browser and access `http://yourdomain.com`. It should load the content from the `yourdomain` directory.
3. Access `http://subdomain.yourdomain.com`. It should load the content from the `subdomain` directory.

Now you have Apache set up with two virtual hosts for a domain and a subdomain on your Windows machine. You can place your website files in the respective directories and customize the virtual host configurations as needed.
