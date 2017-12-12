# wordpress-assets
Wordpress snipets code and tutorials and more information.


Documentation:

https://make.wordpress.org/core/handbook/best-practices/inline-documentation-standards/php/

# General Wordpress
Composer

https://roots.io/using-composer-with-wordpress/

https://deliciousbrains.com/using-composer-manage-wordpress-themes-plugins/

https://roots.io/twelve-factor-wordpress/

## How to work

https://code.tutsplus.com/series/creating-maintainable-wordpress-meta-boxes--cms-661

https://code.tutsplus.com/tutorials/creating-maintainable-wordpress-meta-boxes-the-layout--cms-22208

https://pippinsplugins.com/series/building-a-database-abstraction-layer/

https://wordpress.stackexchange.com/questions/88505/custom-post-type-or-custom-tables

https://wordpress.stackexchange.com/questions/4852/post-meta-vs-separate-database-tables

https://premium.wpmudev.org/blog/creating-database-tables-for-plugins/?imob=b&utm_expid=3606929-106.UePdqd0XSL687behGg-9FA.1&utm_referrer=https%3A%2F%2Fwww.google.es%2F


http://www.danielauener.com/wordpress-post-to-post-relationships-without-altering-the-database/


https://www.smashingmagazine.com/2012/01/create-custom-taxonomies-wordpress/

https://code.tutsplus.com/tutorials/a-guide-to-wordpress-custom-post-types-creation-display-and-meta-boxes--wp-27645

https://pippinsplugins.com/lets-talk-extensible-code/

https://pippinsplugins.com/how-i-built-settings-system-easy-digital-downloads/







https://local.getflywheel.com/

How to start [Beginners guide](wordpress/wordpress_development/development.md).

https://10up.github.io/Engineering-Best-Practices/

https://www.scalewp.io/

https://vip.wordpress.com/documentation/vip/best-practices/

Everything about the [philosophy](https://engineering.hmn.md/how-we-work/philosophy/), [processes](https://engineering.hmn.md/how-we-work/process/), and coding [style](https://engineering.hmn.md/how-we-work/style/) at [Human Made](https://engineering.hmn.md/).

How to [start a plugin](wordpress/wordpress_plugins.md)
How to [publish on wordpress.org](wordpress/wordpress_plugins_publishing.md).

Wordpress at [Stackoverflow](http://stackoverflow.com/documentation/wordpress)

How to [start a widget](wordpress/wordpress_widgets.md)

http://mediatemple.net/resources/web-hosting-101/developing-wordpress-for-the-future/

https://docs.woocommerce.com/document/create-a-plugin/

https://neliosoftware.com/blog/create-better-plugins-with-the-plugin-boilerplate/

[Tutorials](wordpress/wordpress_tutorials.md)

https://carlalexander.ca/organizing-files-object-oriented-wordpress-plugin/

https://tommcfarlin.com/organizing-wordpress-theme-functions/

https://www.codeinwp.com/blog/the-wordpress-plugin-boilerplate-101/

https://codeable.io/how-to-extend-a-wordpress-plugin-my-plugin-boilerplate/ --- https://gist.github.com/LiamBailey/c2ad8a8a1d805f7a2b9d#file-wordpress-plugin-boilerplate-php

https://github.com/washingtonstateuniversity

http://wpcandy.com/teaches/how-to-create-a-functionality-plugin/#.WRyEuMklGRt

https://github.com/BoilWP


## Documentation
https://tools.ietf.org/rfcdiff

### Comenting PHP

[PSR-5: PHPDoc](https://github.com/phpDocumentor/fig-standards/blob/master/proposed/phpdoc.md) from [php-fig](http://www.php-fig.org/psr/).

### Readme Template

Wordpress [Readme Templates](readme/README.md) in .md and .txt format.

#### Comenting md
* [Mastering Markdown](https://guides.github.com/features/mastering-markdown/) by [GitHub](https://guides.github.com/).
* [WordPress Markdown](https://daringfireball.net/projects/markdown/).
* [Markdown Editor](https://stackedit.io/editor)

#### Readme Bibliography

* [Wordpres readme.txt](readme/readme.txt) from [wordpress readme](https://wordpress.org/plugins/developers/#readme "wordpress.org"), [wordpress txt](https://developer.wordpress.org/plugins/wordpress-org/how-your-readme-txt-works/) and [smashingmagazine](https://www.smashingmagazine.com/2011/11/improve-wordpress-plugins-readme-txt/ "smashingmagazine.com").

* [Changelog](readme/docs/changelog/CHANGELOG.md) from [keepachangelog 3.0](http://keepachangelog.com/en/0.3.0/ "keepachangelog.com/en/0.3.0") and [codex.wordpress](https://codex.wordpress.org/Version_4.7 "codex.wordpress.org").

* Semantic Versioning Specification or [semver](readme/docs/semver/semver.md) from [semver](http://semver.org "semver.org").

* [Contributing](readme/docs/contributing/CONTRIBUTING.md) from [contributor-covenant](http://contributor-covenant.org/ "contributor-covenant.org").

# PressBooks
* Pressbooks [Open Source project](https://pressbooks.org/).
* Pressboks [Documentantion](http://docs.pressbooks.org/).
* Pressbooks Code on [GitHub](http://github.com/pressbooks/pressbooks/).
* Pressbooks guidelines for [Contributing](https://github.com/pressbooks/pressbooks/blob/dev/.github/CONTRIBUTING.md).

## PB Plugins
[PB TextBooks](https://github.com/BCcampus/pressbooks-textbook) by [BCcampus](https://github.com/BCcampus).
[PB mPDF](https://github.com/BCcampus/pressbooks-mpdf) by [BCcampus](https://github.com/BCcampus).

# General Theory

### Contributions

https://opensource.guide/how-to-contribute/#how-to-submit-a-contribution.


### Projects
http://readyset.tigris.org/nonav/templates/index.html



Downloads: plugins

https://github.com/washingtonstateuniversity/WSUWP-TOC-Generator   ---  http://projects.jga.me/toc/



# Editting .htaccess files
## Changes in Wordpress main folder
# END WordPress
# Protection https://premium.wpmudev.org/blog/htaccess
# http://www.wpexplorer.com/htaccess-wordpress-security/


# Protecting Important Files
<FilesMatch "^.*(error_log|wp-config\.php|php.ini|\.[hH][tT][aApP].*)$">
Order deny,allow
Deny from all
</FilesMatch>

# Prevent Directory Browsing
Options All -Indexes

# Restrict Access to PHP Files
RewriteCond %{REQUEST_URI} !^/wp-content/plugins/file/to/exclude\.php
RewriteCond %{REQUEST_URI} !^/wp-content/plugins/directory/to/exclude/
RewriteRule wp-content/plugins/(.*\.php)$ - [R=404,L]
RewriteCond %{REQUEST_URI} !^/wp-content/themes/file/to/exclude\.php
RewriteCond %{REQUEST_URI} !^/wp-content/themes/directory/to/exclude/
RewriteRule wp-content/themes/(.*\.php)$ - [R=404,L]


# Protect Your Site Against Script Injections
Options +FollowSymLinks
RewriteEngine On
RewriteCond %{QUERY_STRING} (<|%3C).*script.*(>|%3E) [NC,OR]
RewriteCond %{QUERY_STRING} GLOBALS(=|[|%[0-9A-Z]{0,2}) [OR]
RewriteCond %{QUERY_STRING} _REQUEST(=|[|%[0-9A-Z]{0,2})
RewriteRule ^(.*)$ index.php [F,L]


# Securing the wp-includes Directory
<IfModule mod_rewrite.c>
RewriteEngine On
RewriteBase /
RewriteRule ^wp-admin/includes/ - [F,L]
RewriteRule !^wp-includes/ - [S=3]
RewriteRule ^wp-includes/[^/]+\.php$ - [F,L]
RewriteRule ^wp-includes/js/tinymce/langs/.+\.php - [F,L]
RewriteRule ^wp-includes/theme-compat/ - [F,L]
</IfModule>


# Prevent Username Enumeration
RewriteCond %{QUERY_STRING} author=d
RewriteRule ^ /? [L,R=301]


# Require SSL
# SSLOptions +StrictRequire
# SSLRequireSSL
# SSLRequire %{HTTP_HOST} eq "www.you-site.com"
# ErrorDocument 403 https://www.your-site.com


# Block WordPress xmlrpc.php requests
# This file allows third-party apps to connect to your WordPress site. if you are not using any third party apps should disable this feature.
<Files xmlrpc.php>
order deny,allow
deny from all
</Files>

## Changes in wp-content folder
# Disable access to all file types except the following
Order deny,allow
Deny from all
<Files ~ ".(xml|css|js|jpe?g|png|gif|pdf|docx|rtf|odf|zip|rar)$">
Allow from all
</Files>

## Changes in uploads folder
# Restrict PHP File Execution
<Directory "/var/www/wp-content/uploads/">
<Files "*.php">
Order Deny,Allow
Deny from All
</Files>
</Directory>



http://www.wpuniversity.com/lesson/wordpress-front-end-vs-back-end





# How to clean the database:

Several plugins can help for cleaning the database (Remember to have a backup of the installation before to start)

## Why WP-Sweep and not WP-Optimize?

You may be wondering why are we writing using WP-Sweep when there is a very popular WP-Optimize plugin available that does nearly the same thing. Because the main distinguishing characteristic of WP-Sweep is that it uses proper WordPress delete functions as much as possible instead of running direct delete MySQL queries. Whereas the WP-Optimize plugin uses direct delete SQL queries which can leave orphaned data left behind.
