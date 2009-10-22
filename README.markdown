MysqlSetup
==========

This is a plugin to do one simple thing: setup your mysql databases and create a user with permissions to access them.  It does this via a rake task.  So here's how you use it:

Warning
-------
This will drop any existing databases defined in your database.yml file.  Don't run it if you have any important data that isn't backed up!!!

Installation
------------
Easy:

	./script/plugin install git://github.com/mlightner/mysql_setup.git


Prerequisites
-------------
Before you can use this plugin, you must have your system's root user's .my.cnf setup with the proper login and password as described [here](http://www.modwest.com/help/kb6-242.html).

Specifically, you should be able to type "mysql" as root and be taken to the mysql command prompt without having to type a password.  If you want to leave it there after using the plugin is up to you and your security policies.


Setup
-----
Create **config/database.yml** (you must be using mysql as your engine).  Be sure to use the same username and password on all of your database configurations, however use a different database name for development, test and production, obviously.  Here's an example:

	defaults: &defaults
	  adapter: mysql
	  username: application_user
	  password: yoursupersecretpass
	  pool: 5
	  timeout: 5000

	development:
	  <<: *defaults
	  database: yourapp_development

	test:
	  <<: *defaults
	  database: yourapp_test

	production:
	  <<: *defaults
	  database: yourapp_production


And, Action!
------------
Simply type the following from the root directory of your app and everything should be setup correctly:

	rake mysql_setup:full

Your output should have no errors.  You're ready to rock and roll.  Here's some sample output:

	you@yourserver [~/yourapp]# sudo rake mysql_setup:full
	Executing: DROP DATABASE IF EXISTS yourapp_production
	Executing: CREATE DATABASE yourapp_production
	Executing: DROP DATABASE IF EXISTS yourapp_development
	Executing: CREATE DATABASE yourapp_development
	Executing: DROP DATABASE IF EXISTS
	Executing: CREATE DATABASE
	Executing: DROP DATABASE IF EXISTS yourapp_test
	Executing: CREATE DATABASE yourapp_test
	Executing: DROP USER 'application_user'
	Executing: FLUSH PRIVILEGES
	Executing: CREATE USER 'application_user' IDENTIFIED BY 'yoursupersecretpass'
	Executing: GRANT ALL PRIVILEGES ON yourapp_production.* TO 'application_user'@'%' IDENTIFIED BY 'yoursupersecretpass'
	Executing: GRANT ALL PRIVILEGES ON yourapp_production.* TO 'application_user'@'localhost' IDENTIFIED BY 'yoursupersecretpass'
	Executing: GRANT ALL PRIVILEGES ON yourapp_production.* TO 'application_user'@'yourserver.yourdomain.com' IDENTIFIED BY 'yoursupersecretpass'
	Executing: GRANT ALL PRIVILEGES ON yourapp_development.* TO 'application_user'@'%' IDENTIFIED BY 'yoursupersecretpass'
	Executing: GRANT ALL PRIVILEGES ON yourapp_development.* TO 'application_user'@'localhost' IDENTIFIED BY 'yoursupersecretpass'
	Executing: GRANT ALL PRIVILEGES ON yourapp_development.* TO 'application_user'@'yourserver.yourdomain.com' IDENTIFIED BY 'yoursupersecretpass'
	Executing: GRANT ALL PRIVILEGES ON .* TO 'application_user'@'%' IDENTIFIED BY 'yoursupersecretpass'
	Executing: GRANT ALL PRIVILEGES ON .* TO 'application_user'@'localhost' IDENTIFIED BY 'yoursupersecretpass'
	Executing: GRANT ALL PRIVILEGES ON .* TO 'application_user'@'yourserver.yourdomain.com' IDENTIFIED BY 'yoursupersecretpass'
	Executing: GRANT ALL PRIVILEGES ON yourapp_test.* TO 'application_user'@'%' IDENTIFIED BY 'yoursupersecretpass'
	Executing: GRANT ALL PRIVILEGES ON yourapp_test.* TO 'application_user'@'localhost' IDENTIFIED BY 'yoursupersecretpass'
	Executing: GRANT ALL PRIVILEGES ON yourapp_test.* TO 'application_user'@'yourserver.yourdomain.com' IDENTIFIED BY 'yoursupersecretpass'

Author and License
------------------
Copyright (c) 2009 Matt Lightner, mlightner@gmail.com, and released under the MIT license.

