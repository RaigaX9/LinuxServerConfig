# LinuxServerConfig

## Introduction

This is a project for the Full Stack Web Developer Nanodegree
where we have to create a Linux Server to run a Flask application.
In this case, it will be from the item catalog project that will
be in here.

## Technologies
- Python 2.7.x
- Flask
- SQLAlchemy
- PostgreSQL
- Amazon AWS


## Instructions

1. Download Private Key from [here](https://www.udacity.com/account#!/development_environment).
2. Move the private key file into the folder ~/.ssh (where ~ is your environment's home directory). So if you downloaded the file to the Downloads folder, just execute the following command in your terminal.
`mv ~/Downloads/udacity_key.rsa ~/.ssh/`
3. Open your terminal and type in `chmod 600 ~/.ssh/udacity_key.rsa
4. In the terminal, type `ssh -i ~/.ssh/udacity_key.rsa root@xx.xxx.xx.xxx
5. After logging in, do `sudo adduser grader` and type in the information.
For mine, I did everything "aaa" when filling out the information.
6. Do `sudo nano /etc/sudoers.d/grader` and write `grader ALL=(ALL) NOPASSWD:ALL`
7. Create ssh folder in grader which is `mkdir /home/grader/.ssh`
8. Do `chmod 700 .ssh`
9. Open another terminal in your local machine and type `ssh-keygen`
10. When filling out the keygen, enter the path for where it will be generated.
So it will be somewhere in `C:\Users\<name>\.ssh` and using that path. Passphrase should be blank.
11. After that, go to id_rsa.pub and copy that key.
12. Go back to your other terminal and do `nano /home/grader/.ssh/authorized_keys`
and paste that key you've copied in there.
13. Do `chmod 644 .ssh/authorized_keys`
14. Now, we can log into as grader which can be done by doing `su - grader`
15. After logging in as grader, install the following:
    
    - `sudo apt-get update`
    - `sudo apt-get upgrade`
    - `sudo apt-get install apache2`
    - `sudo apt-get install libapache2-mod-wsgi`
    - `sudo apt-get install postgresql`
    - `sudo apt-get install ntp`
    - `sudo apt-get install python-pip`
    - `sudo apt-get install python-psycopg2`
    - `sudo pip install flask`
    - `sudo pip install oauth2client`
    - `sudo pip install sqlalchemy`
    - `sudo apt-get install git`
16. Do `sudo -u postgres -i` to switch to postgres
17. Do `psql`
18. Run `CREATE USER grader WITH PASSWORD 'aaa';`
19. Run `GRANT ALL PRIVILEGES ON DATABASE "catalog" to grader;`
20. Now to get out psql, do `\q`.
21. To get out of postgres, do `exit`
22. Now to change the ports and for that we will go to `sudo nano /etc/ssh/sshd_config`
23. Change `Port 22` to `Port 2200`
24. Change `PermitLogin without-password` to `PermitLoging no`
25. Restart the server by doing `sudo service ssh restart`
26. Next, to configure the firewall, do the following:
    - `sudo ufw default deny incoming`
    - `sudo ufw default allow outgoing`
    - `sudo ufw allow 2200/tcp`
    - `sudo ufw allow www` (for 80)
    - `sudo ufw allow ntp` (for 123)
    - `sudo ufw deny 22` (to not allow 22)
27. After that, do `sudo ufw enable` to start up the firewall. To check
if everything is alright, do `sudo ufw status`.
28. To change the timezone, do `sudo dpkg-reconfigure tzdata` and select
`None of the Above` and `UTC`
29. To configure Apache, do `sudo nano /etc/apache2/sites-enabled/000-default.conf` and
fill in the following inside VirtualHost:

        ServerName 35.164.164.97
        ServerAdmin webmaster@localhost
        ServerAlias ec2-35-164-164-97.us-west-2.compute.amazonaws.com
        DocumentRoot /var/www/html/ItemCatalog/vagrant/templates/
        WSGIScriptAlias / /var/www/html/catalog.wsgi
30. Next, do `sudo nano /var/www/html/catalog.wsgi` and fill
the following below:

        import sys
        sys.path.insert(0, '/var/www/html/ItemCatalog/vagrant/')
        from project import app as application
        application.secret_key = 'falcon pawnch'
31. Do `cd /var/www/html` and git clone the project: 
`sudo git clone https://github.com/ZetaX9/ItemCatalog.git`
32. In `databasesetup.py`, `insertdata.py`, and `project.py`,
change sqllite database information to `postgresql+psycopg2://grader:aaa@localhost:5432/catalog`
33. Then change the Google Authentication to `http://ec2-35-164-164-97.us-west-2.compute.amazonaws.com/`
in both Authorized Javascript Origins and Authorized redirect URIs as well as in `client_secrets.json` in the project folder.
34. Last, do `sudo service apache2 restart ` and the application will be LIVE!

## Live Flask Application

[http://ec2-35-164-164-97.us-west-2.compute.amazonaws.com/](http://ec2-35-164-164-97.us-west-2.compute.amazonaws.com/)

## References

- Udacity - Linux Lecture Videos and Assignments
- [http://httpd.apache.org/docs/trunk/urlmapping.html](http://httpd.apache.org/docs/trunk/urlmapping.html)
- [http://askubuntu.com/questions/138423/how-do-i-change-my-timezone-to-utc-gmt](http://askubuntu.com/questions/138423/how-do-i-change-my-timezone-to-utc-gmt)
- [http://stackoverflow.com/questions/31631731/use-a-git-repository-on-var-www-html](http://stackoverflow.com/questions/31631731/use-a-git-repository-on-var-www-html)
- [https://www.digitalocean.com/community/tutorials/how-to-create-a-sudo-user-on-ubuntu-quickstart](https://www.digitalocean.com/community/tutorials/how-to-create-a-sudo-user-on-ubuntu-quickstart)
- [https://www.digitalocean.com/community/questions/discussion-about-permissions-for-web-folders](https://www.digitalocean.com/community/questions/discussion-about-permissions-for-web-folders)
