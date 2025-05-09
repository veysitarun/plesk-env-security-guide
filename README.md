  Plesk .env Security - Quick Guide

📎 Plesk .env Security - Quick Guide
====================================

This guide explains step-by-step the precautions you need to take to secure the `.env` file in web projects running on Plesk. If accessible from the outside, the `.env` file poses a major security risk. You’ll find applicable solutions for both **NGINX** and **Apache** web servers in this guide.

🔒 Problem Description
----------------------

*   The `.env` file contains critical configuration information (database passwords, API keys).
*   If it is accessible externally, this information can be stolen by malicious actors.
*   If the `.env` file is **downloadable**, it can lead to serious security threats.

* * *

✅ Solution: Blocking Access via NGINX
-------------------------------------

### 1\. Editing the NGINX Configuration

You can block external access to the `.env` file by adding extra directives to NGINX through the Plesk panel.

#### Steps:

1.  Log in to the Plesk Panel.
2.  Go to the Website > Apache & nginx Settings tab.
3.  Add the following code to the **Additional nginx directives** section:

    location ~* /\.env$ {
        return 404;
    }

4.  Save and restart NGINX.

### 2\. Testing

Use incognito mode in your browser to test the access:

    curl -I https://domain.com/.env

If you receive **HTTP/1.1 404 Not Found** as a response, the configuration was successful.

* * *

✅ Solution: Blocking Access via Apache
--------------------------------------

### 1\. Configuring Apache

If you’re using Apache, you can block requests to the `.env` file by editing the `.htaccess` file.

#### Steps:

1.  In the Plesk Panel, go to Website > Apache & nginx Settings tab.
2.  Under the Apache Web Server section, add the following directive to the `.htaccess` file:

    <FilesMatch "^\.env$">
        Order allow,deny
        Deny from all
    </FilesMatch>

3.  Save and restart Apache.

### 2\. Testing

Test the access control again using an incognito window:

    curl -I https://domain.com/.env

If the response is **404 Not Found**, the Apache configuration has been successfully applied.

* * *

⚠️ Points to Consider
---------------------

*   **Moving the file**: Moving the `.env` file to another directory may prevent frameworks like Strapi from reading it. This is a temporary workaround and might cause configuration errors.
*   **Proxy Mode**: If Proxy Mode is disabled in Plesk, NGINX is used directly. In this case, Apache settings won’t be effective, and you’ll need to use NGINX configuration.
*   **Browser cache**: If you still see the old configuration after changes, try clearing your browser cache or test in incognito mode.

* * *

🧪 Test Commands
----------------

### 1\. Access Control Test

    curl -I https://domain.com/.env

### Expected Response:

    HTTP/1.1 404 Not Found
