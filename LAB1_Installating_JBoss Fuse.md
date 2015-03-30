LAB 01 – INSTALLING JBOSS FUSE


Installing JBoss Fuse 6 is easy. You'll need to unzip the distribution into the target directory. 
You can get a JBoss Fuse distribution from http://www.jboss.org/products/fuse.

```
[lab9@machine ~]# mkdir Servers
[lab9@machine ~]# cd Servers
[lab9@machine Servers]# unzip ~/Downloads/jboss-fuse-full-6.1.0.redhat-379.zip
```
This will create a subdirectory named jboss-fuse-6.1.0.redhat-379.

You will need to edit a couple of configuration files after your first install to setup an administrative username and password.

Edit <JBoss Fuse Home>/etc/users.properties, and uncomment (remove the leading #) the last line. 
The format is `<username>=<password>,<role>`. For the purposes of this lab, let's use following:

```
admin=Passw0rd!,admin
```

You will also want to install Fuse IDE, which you can also download from the same jboss.org site, and the
install is equally easy – just double click the `Fuse-IDE-Enterprise-7.1.0.fuse-047-<platform>.exe` file and folow the instraction..

That's it! An install so fast.
