Vagrant box generator
=========

Vagrant box generator allows you to create a vagrant box with all the software needed to build a rails app.

About the system
---

Currently Installed
* Ruby 2.0.0-p247
* Git 1.7.4
* Postgresql 3.3.0
* QT 4 - for headless browser testing
* RVM 1.23.9 - stable version
* NFS - for faster file system sharing

Installation
---

Required software
* Vagrant - http://www.vagrantup.com
* Virtualbox - http://www.virtualbox.org
* Install Librarian-chef gem `gem install librarian-chef`

Steps
---

1. Clone repository
2. Open a terminal app and browse to the cloned repository
3. Run librarian-chef which downloads cookbooks into a cookbooks folder in that directory
        librarian-chef install
4. Bring up the vagrant box - this will download a base box and apply chef scripts to it when it is booting up.
        vagrant up
5. SSH into your newly created box
        vagrant ssh
6. Fix postgresql UTF8 issue - http://ezekielbinion.com/blog/making-rake-dbcreate-postgres-behave/
        psql -U postgres -h localhost

    Run this command inside of psql

        UPDATE pg_database SET datallowconn = TRUE where datname = 'template0';
        \c template0
        UPDATE pg_database SET datistemplate = FALSE where datname = 'template1';
        drop database template1;
        create database template1 with template = template0 encoding = 'UNICODE'  LC_CTYPE = 'en_US.UTF-8' LC_COLLATE = 'C';
        UPDATE pg_database SET datistemplate = TRUE where datname = 'template1';
        \c template1
        UPDATE pg_database SET datallowconn = FALSE where datname = 'template0';