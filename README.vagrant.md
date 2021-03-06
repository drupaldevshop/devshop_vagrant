DevShop Vagrant
===============

This project contains a Vagrantfile for launching a devshop virtual machine.

It uses the stock [install-ubuntu.sh](https://github.com/drupaldevshop/devshop/blob/6.x-1.x/install.debian.sh)
file in the devshop project so that deployed servers and vagrant boxes are the same.

Dependencies
------------

- Git
- Vagrant 1.5.x
- VirtualBox (or any supported virtualization software like VMWare)

Setup
-----

To get started with this project, you need to setup the right versions of it's dependencies.

Some of you may have these things, so just make sure they are up to date.

It is best to use the installers from the websites so you are sure to get the right version.


1. Install Vagrant.
  Available at [http://www.vagrantup.com/downloads.html](http://www.vagrantup.com/downloads.html).
  
  *Use version 1.5.x and up.*

2. Install VirtualBox.
  Available at [https://www.virtualbox.org/wiki/Downloads](https://www.virtualbox.org/wiki/Downloads)
  
  *Use version 4.3.x and up.*

3. Install Git.
  Available at [http://www.gitscm.com/](http://www.gitscm.com/)

  *Any version is probably fine.*

5. Clone this Repo.

  ```
  git clone git@github.com:drupaldevshop/devshop_vagrant.git
  ```

Usage
-----

Once that's all done you can launch (and destroy) the devshop VM any time you wish with:

  ```
  cd devshop_vagrant
  vagrant up
  ```
Once up, you can ssh into the VM with:
  ```
  vagrant ssh
  ```

### DevShop Installer

The first time you `vagrant up` it will install devshop.  Once the install script finished you should 
see the stock DevShop Install Success output with a link to login to your devshop front-end.

*You must use that link to login.* The admin password is automatically generated.
  
See "DevShop Management" for instructions for accessing the server to do things like
reset the password.

DevShop Management
------------------

  To access the server simply use `vagrant ssh`, then switch to the `aegir` user:
  ```
  sudo su - aegir
  ```
  Once you are the aegir user, you can interact with the devshop front-end with drush.  For example, 
  to get another login link, use the `drush uli` command:
  ```
  drush @hostmaster uli
  ```
  All server-side interactions with your sites are done as the `aegir` user.
  
  To see all hosted sites call 
  ```
  drush site-alias
  ```
  
*NOTE: When you create new projects and environments, you will need to add those URIs to your 
hosts file as well, or you will not be able to access them.*

Development
-----------

If you are interested in developing devshop, there are a few more steps you need to take.

  1. `vagrant up` with the synced folder commented out.
  2. Uncomment line 45 in `Vagrantfile` (the line with config.vm.synced_folder), and run `vagrant reload`.
  3. Upon reload, mysql and apache may stop for some reason. Vagrant ssh in and
     `sudo service mysql restart` and `sudo service apache2 restart`

The reasoning here is that the synced_file must be owned by `aegir`, but the user doesn't exist until
the provisioner runs.  There is currently now way around this problem, that I know of.
