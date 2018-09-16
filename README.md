# svn
To install SVN repo on redhat7 

####################################################################
============================================================================
-------------------------------------- SVN ---------------------------------
============================================================================
rpm -Uvh https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
yum install -y update ansible git maven


Step 1 . Install Apache and SVN
-----------------------
yum -y install httpd mod_dav_svn httpd-devel svn
systemctl status httpd
systemctl start httpd

step 2 - enable module
--------------------
a2enmod dav
a2enmod dav_svn
service apache2 restart

Step 3 . Configure Apache with Subversion
------------------------------------------
vim /etc/httpd/conf.d/svn.conf
Alias /svn /app/svn
<Location /svn>
   DAV svn
   SVNParentPath /app/svn
   AuthType Basic
   AuthName "Subversion Repository"
   AuthUserFile /etc/httpd/dav_svn.passwd
   Require valid-user
</Location>

Step 4 . Create First SVN Repository
------------------------------------
mkdir -p /app/svn/
svnadmin create /app/svn/myrepo
chown -R apache:apache /app/svn
chmod -R 775 /app/svn/

Step 5 . Create Users for Subversion
------------------------------------
htpasswd -cm /etc/httpd/dav_svn.passwd admin

To create additional users, use following commands.
---------------------------------------------------
htpasswd -m /etc/httpd/dav_svn.passwd user1
htpasswd -m /etc/httpd/dav_svn.passwd user2

Step 6 . Access Repository in Browser
http://example.com/svn/myrepo/


Below are the defied variables
------------------------------------------
  SVN_Parent_Path: "/app/svn"
  svn_url_location: "svn"
  svn_admin: "admin"
  svn_adminpass: "admin"
  svn_user1: "user1"
  svn_user1pass: "test123"
  svn_user2: "user2"
  svn_user2pass: "test123"

