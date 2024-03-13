# WordPress

- Installation
- Theme development
- Publishing and updating content
- Plugins

---

## Terminology

- **CMS** - Content Management System
  - A software application that can be used to manage the creation and modification of digital content.
  - Typically used for enterprise content management and web content management.
  - Version control, indexing, search, and retrieval are some of the features of a CMS.
  - Examples: WordPress, Joomla, Drupal, Magento, Shopify, Squarespace, Wix, Weebly, etc.

---

- **LAMP** - Linux, Apache, MySQL, PHP
  - A software stack for developing and deploying web applications.
  - **Linux** is the operating system
  - **PHP** - Hypertext Preprocessor
    - A server-side scripting language designed for web development
  - **MySQL** - Open-source relational database management system
  - **Apache** - Open-source web server software

---

- **WordPress** - Open-source content management system
  - Originally created as a blog-publishing system. Today, it is used for a wide variety of websites. Because it was
    originally created for blogging, the terminology comes from the world of blogging.
  - Written in PHP
  - Uses MySQL as a database
  - Most popular CMS
  - Features can be extended with plugins and themes.
    - Popular plugins: WooCommerce, Yoast SEO, Contact Form 7, Elementor, Anti-Spam, Jetpack, etc.
  - Usage statistics: [W3Techs](https://w3techs.com/technologies/details/cm-wordpress)
  - [WordPress.org](https://wordpress.org/)

---

- **Page** - A static page that is not a part of the blog feed. It is a standalone page that is not affected by time. In
  websites, pages are used for static content that is not updated often like "About us", "Contact us", etc.
- **Post** - A blog post that is part of the blog feed. It is affected by time and is displayed in reverse chronological
  order. In websites, posts are used for dynamic content that is updated often like news, articles, products, etc.
- **Category** - A way to group posts that are related to each other. It is a method to organize content.
- **Tag** - A keyword that is used to describe
  the content of a post. 
- **Theme** - A collection of templates and stylesheets used to define the appearance and display of a WordPress powered
  website.
- **Template** - A file that controls the layout of a WordPress website. It is a file that is used to generate the final
  HTML output.
- **Plugin** - A piece of software containing a group of functions that can be added to a WordPress website. They can
  extend functionality or add new features to your WordPress websites.
- **Sidebar** - A widget-ready area in a WordPress theme that is used to display information that is not a part of the
  main
  content.
- **Widget** - A small block that performs a specific function. You can add these widgets in sidebars also known as
  widget-ready areas on your web page.

---

## Installation

- **Local development environment**
  - [XAMPP](https://www.apachefriends.org/download.html), [MAMP](https://www.mamp.info/en/downloads/) etc.
- **Metropolia server**
  - [Instructions](https://metropoliafi-my.sharepoint.com/:v:/g/personal/ilkkamtk_metropolia_fi/EQxwMIi9hJlLi_675cMc2hgBy858Xt0qwVSmIK8d7YCAfA?nav=eyJyZWZlcnJhbEluZm8iOnsicmVmZXJyYWxBcHAiOiJPbmVEcml2ZUZvckJ1c2luZXNzIiwicmVmZXJyYWxBcHBQbGF0Zm9ybSI6IldlYiIsInJlZmVycmFsTW9kZSI6InZpZXciLCJyZWZlcnJhbFZpZXciOiJNeUZpbGVzTGlua0NvcHkifX0&e=XiWJOI)
- **Official guide:
  ** [WordPress Codex](https://developer.wordpress.org/advanced-administration/before-install/howto-install/)
- **Download** WordPress from [fi.wordpress.org](https://fi.wordpress.org/) for Finnish version
  or [wordpress.org](https://wordpress.org/) for English version.
- **Installation**

  - Unzip the downloaded file
  - Move the extracted folder to the desired location
    - If you are using XAMPP or MAMP, move the folder to `htdocs` folder
    - If you are using Metropolia's server, move the folder to `public_html` folder
  - Create a database locally or on Metropolia's server
    - [Metopolia database setup](https://amme.metropolia.fi/mysql/)
    - You need to remember the database address, name, username, and password
    - **Local settings:**
      - address: `localhost`
      - database name: `whatever you used`
      - username: `root`
      - password: ``
    - **Metropolia's server settings:**
      - address: `mysql.metropolia.fi`
      - database name: `your username`
      - username: `your username`
      - password: `your DATABASE password`
  - Open the WordPress installation in a browser and follow the instructions
    - You have to open the files through a web server. You can use XAMPP, MAMP, or any other local server software
      or Metropolia's server.
    - localhost: `http://localhost/your-folder-name`
    - Metropolia's server: `http://users.metropolia.fi/~your-username/your-folder-name`

You can opt in to your WordPress app receiving automatic updates. If not, you can update it manually from the admin panel. But, keep in mind
that you should always have a backup of your website before updating and never update a production website without
testing the update on a development environment.

---

## Uploads on Metropolia's server
  - `.htaccess` on Metropolia's server to allow file uploads:

    - You need to create a `.htaccess` file with a proper editor (like Visual Studio Code, etc.)

      ```apacheconfig
      AddHandler cgi-script .php
      ```

    - Save the file and upload it to the root folder of your WordPress installation.
    - Change the file permissions to 755:
      - Open shell access with your terminal: `ssh username@shell.metropolia.fi`
      - Navigate to your public folder: `cd public_html`
      - Change the permissions of all files in your WordPress installation: `chmod -R 755 *` so that the server
        can read and execute the files.

---

## Wordpress Settings

- Video: [WordPress Settings](https://metropoliafi-my.sharepoint.com/:v:/g/personal/ilkkamtk_metropolia_fi/ETRDffBPuydIiNgMcxoKjh4B5tFTx96C46yp2PJPNEexkw?nav=eyJyZWZlcnJhbEluZm8iOnsicmVmZXJyYWxBcHAiOiJPbmVEcml2ZUZvckJ1c2luZXNzIiwicmVmZXJyYWxBcHBQbGF0Zm9ybSI6IldlYiIsInJlZmVycmFsTW9kZSI6InZpZXciLCJyZWZlcnJhbFZpZXciOiJNeUZpbGVzTGlua0NvcHkifX0&e=MCEsuX)

---

## Wordpress plugins

WordPress plugins are used to extend the functionality of your website. There are thousands of free and paid plugins available. For example, you can use plugins to add contact forms, galleries, sliders, e-commerce, SEO, etc.

You can install and activate plugins from the admin panel.

### SSH Updater Plugin

On Metropolia's server you can't update your WordPress installation or install plugins from the admin panel. You need to use SSH to update your WordPress installation and install plugins. You can use the SSH Updater plugin to update your WordPress installation and install plugins.

- Download the plugin from https://wordpress.org/plugins/ssh-sftp-updater-support/
- Unzip the downloaded file and move the `ssh-sftp-updater-support` folder to the `wp-content/plugins` folder
- Activate the plugin from the admin panel / plugins

---

## Assignment

1. Remove the default content and default plugins from your WordPress installation.
2. Add `ssh-sftp-updater-support` plugin to your WordPress installation.
3. Add two new pages 'Home' and 'About Us' to your WordPress installation. The pages should have a title and some text content.
4. Set the 'Home' page as the front page of your WordPress installation in the reading settings.
5. Open the front page of your WordPress site and check that everything works.


