# SSL Certificate
Certificate Installation : NGINX
                                                                                                                                                  

Prerequisites:

Concatenate the CAbundle and the certificate file which we sent you using the following command.                                                                  

> cat domain_com.crt domain_com.ca-bundle > ssl-bundle.crt


If you are Using GUI Text Editor (Ex: Notepad):

(i) To concatenate the certificate files into single bundle file, first open domainname.crt and domainname.ca-bundle files using any text editor.

(ii) Now copy all the content of domainname.crt and paste it on the top of domainname.ca-bundle file.

(iii) Now save the file name as ‘ssl-bundle.crt’.

 

Note: If you have not the received the 'ca-bundle' file in the ZIP that we sent you, you can download it from this article's attachments. (End of this page)

 

Installation:


1. Store the bundle in the appropriate nginx ssl folder

EX :

> mkdir -p /etc/nginx/ssl/example_com/

> mv ssl-bundle.crt /etc/nginx/ssl/example_com/


2. Store your private key in the appropriate nginx ssl folder,

EX : 

> mv example_com.key /etc/nginx/ssl/example_com/

 

3. Make sure your nginx config points to the right cert file and to the private key you generated earlier:

server {
listen 443;
server_name domainname.com;
ssl on;
ssl_certificate /etc/ssl/certs/ssl-bundle.crt;
ssl_certificate_key /etc/ssl/private/domainname.key;
ssl_prefer_server_ciphers on;
}

Note: If you are using a multi-domain or wildcard certificate, it is necessary to modify the configuration files for each domain/subdomain included in the certificate. You would need to specify the domain/subdomain you need to secure and refer to the same certificate files in the VirtualHost record the way described above.

 

4. OCSP Stapling Support:

Although optional, it is highly recommended to enable OCSP Stapling which will improve the SSL handshake speed of your website. NginX has OCSP Stapling functionality enabled since version 1.3.7.

In order to use OCSP Stapling in NginX, you must set the following in your configuration:

## OCSP Stapling
resolver 127.0.0.1;
ssl_stapling on;
ssl_stapling_verify on;
ssl_trusted_certificate <file>;

Where <file> is the name location and filename of the certificate installed.

 

Note 1: For ssl_stapling_verify and ssl_stapling to work, you must ensure that all necessary intermediates and root certificates are installed.

Note 2: The resolver name may change based on your environment.

 

5. After making changes to your config file check the file for syntax errors before attempting to use it. The following command will check for errors:

> sudo nginx -t -c /etc/nginx/nginx.conf

 

6. Restart your server. Run the following command to do it:

> sudo /etc/init.d/nginx restart

 

7. To verify if your certificate is installed correctly, use our SSL Analyzer.

 

Example Virtual Host Configuration:

server {
    listen 80 default_server;
    listen [::]:80 default_server;

    # Redirect all HTTP requests to HTTPS with a 301 Moved Permanently response.
    return 301 https://$host$request_uri;
}

server {
    listen 443 ssl http2;
    listen [::]:443 ssl http2;

    # certs sent to the client in SERVER HELLO are concatenated in ssl_certificate
    ssl_certificate /path/to/signed_cert_plus_root_plus_intermediates;
    ssl_certificate_key /path/to/private_key;
    ssl_session_timeout 1d;
    ssl_session_cache shared:SSL:50m;
    ssl_session_tickets off;

    # Diffie-Hellman parameter for DHE ciphersuites, recommended 2048 bits
    ssl_dhparam /path/to/dhparam.pem;

    # intermediate configuration.
    ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
    ssl_ciphers 'ECDHE-ECDSA-CHACHA20-POLY1305:ECDHE-RSA-CHACHA20-POLY1305:ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-GCM-SHA384:DHE-RSA-AES128-GCM-SHA256:DHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-AES128-SHA256:ECDHE-RSA-AES128-SHA256:ECDHE-ECDSA-AES128-SHA:ECDHE-RSA-AES256-SHA384:ECDHE-RSA-AES128-SHA:ECDHE-ECDSA-AES256-SHA384:ECDHE-ECDSA-AES256-SHA:ECDHE-RSA-AES256-SHA:DHE-RSA-AES128-SHA256:DHE-RSA-AES128-SHA:DHE-RSA-AES256-SHA256:DHE-RSA-AES256-SHA:ECDHE-ECDSA-DES-CBC3-SHA:ECDHE-RSA-DES-CBC3-SHA:EDH-RSA-DES-CBC3-SHA:AES128-GCM-SHA256:AES256-GCM-SHA384:AES128-SHA256:AES256-SHA256:AES128-SHA:AES256-SHA:DES-CBC3-SHA:!DSS';
    ssl_prefer_server_ciphers on;

    # HSTS (ngx_http_headers_module is required) (15768000 seconds = 6 months)
    add_header Strict-Transport-Security max-age=15768000;

    # OCSP Stapling ---
    # fetch OCSP records from URL in ssl_certificate and cache them
    ssl_stapling on;
    ssl_stapling_verify on;

    ## verify chain of trust of OCSP response using Root CA and Intermediate certs
    ssl_trusted_certificate /path/to/root_CA_cert_plus_root_plus_intermediates;

    resolver <IP DNS resolver>;

    ....
}
 

* If you want a more pre-defined, secure configuration, please check Mozilla SSL Configuration Generator

