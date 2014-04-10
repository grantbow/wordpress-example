Wordpress with Etherpad on OpenShift
====================================

This git repository helps you get up and running quickly w/ a Wordpress with Etherpad
installation on OpenShift.  The backend database is MySQL and the database name is the 
same as your application name (using getenv('OPENSHIFT_APP_NAME')).  You can name
your application whatever you want.  However, the name of the database will always
match the application so you might have to update .openshift/action_hooks/build.

Running on OpenShift
----------------------------

Create an account at https://www.openshift.com and install the client tools (run 'rhc setup' first)

Create a php-5.4 application (you can call your application whatever you want)

    rhc app create wordpress php-5.4 nodejs-0.6 mysql-5.5 mongodb-2 --from-code=https://github.com/grantbow/wordpress-example

That's it, you can now checkout your application at:

    http://wordpress-$yournamespace.rhcloud.com
    
You'll be prompted to set an admin password and name your WordPress site the first time you visit this 
page.  The default etherpad admin account is admin/OpenShiftAdmin.

Note: When you upload plugins and themes, they'll get put into your OpenShift data directory
on the gear ($OPENSHIFT_DATA_DIR).  If you'd like to check these into source control, download the 
plugins and themes directories and then check them directly into php/wp-content/themes, php/wp-content/plugins.

etherpad git push warnings:
    You can safely ignore these. However if they really annoy you, you can
    edit settings.json and replace the database settings with your own.
    Then edit .openshift/action_hooks/deploy and remove the sed statements.

Notes
=====

GIT_ROOT/.openshift/action_hooks/deploy:
    This script is executed with every 'git push'.  Feel free to modify this script
    to learn how to use it to your advantage.  By default, this script will create
    the database tables that this example uses.

    If you need to modify the schema, you could create a file 
    GIT_ROOT/.openshift/action_hooks/alter.sql and then use
    GIT_ROOT/.openshift/action_hooks/deploy to execute that script (make sure to
    back up your application + database w/ 'rhc app snapshot save' first :) )

Security Considerations
-----------------------
Consult the WordPress documentation for best practices regarding securing your wordpress installation.  OpenShift 
automatically generates unique secret keys for your deployment into wp-config.php, but you may feel more
comfortable following the WordPress documentation directly.

