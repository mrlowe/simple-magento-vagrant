simple-magento-vagrant
======================

# Site Dump

To use this fork as a site dump for testing or debugging a mysterious production site, do the following:

* Put the magento install tarball in a new httpdocs folder and make sure the version matches.
* Pull down the site database as an sql dump. Say the database is named foo and the outfile is foo.sql.
* Pull down the /media and /skins directories as well.
* Pull down the site's local.xml from /app/etc/local.xml
* Put all that stuff into a foo-data directory.
* Archive foo-data into a tarball, like ```tar cvzf foo-data.tar.gz foo-data```
* Make a copy of bootstrap-vars-template.sh and modify variables in bootstrap-vars.sh with the data from the local.xml
* If necessary, modify tweaks.sql in the vagrant root folder to modify data in the database, like the secure and unsecure host names.
* Run ```vagrant up``` to create the vagrant box
* Put the site's local.xml in httpdocs/app/etc/
* Hit http://localhost:8080 to see if you win!
* (If you get a numbered error, look in httpdocs/var/report/<number> to see what's up.)
* TODO: modules and custom layouts won't work unless you also copy down the stuff in:
/app/design/frontend/<design name> as well as /app/code/local and any modules loaded in /app/etc/modules 
* Also any plugins will probably live in /app/code/community.

# The Standard Stuff

A VERY simple Magento environment provisioner for [Vagrant](http://www.vagrantup.com/).

![Magento & Vagrant](https://cookieflow.files.wordpress.com/2013/07/magento_vagrant.jpg?w=525&h=225)

* Creates a running Magento development environment with a few simple commands.
* Runs on Ubuntu (Trusty 14.04 64 Bit) \w PHP 5.5, MySQL 5.5, Apache 2.2
* Uses [Magento CE 1.9.1.0](http://www.magentocommerce.com/download)
* Automatically runs Magento's installer and creates CMS admin account.
* Optionally installs Magento Sample Store Inventory
* Automatically runs [n98-magerun](https://github.com/netz98/n98-magerun) installer.
* Perfect for rapid development or extension testing with an unopionionated, bare-bones and easily tweaked configuration.
* Goes from naught-to-Magento in a couple of minutes.

## Getting Started

**Prerequisites**

* Install [VirtualBox](https://www.virtualbox.org/wiki/Downloads)
* Install [Vagrant](http://www.vagrantup.com/)
* Clone or [download](https://github.com/r-baker/simple-magento-vagrant/archive/master.zip) this repository to the root of your project directory `git clone https://github.com/r-baker/simple-magento-vagrant.git`
* In your project directory, run `vagrant up`

The first time you run this, Vagrant will download the bare Ubuntu box image. This can take a little while as the image is a few-hundred Mb. This is only performed once.

Vagrant will configure the base system before downloading Magento and running the installer.

## Usage

* In your browser, head to `127.0.0.1:8080`
* Magento CMS is accessed at `127.0.0.1:8080/admin`
* User: `admin` Password: `password123123`
* Access the virtual machine directly using `vagrant ssh`
* When you're done `vagrant halt`

[Full Vagrant command documentation](http://docs.vagrantup.com/v2/cli/index.html)

## Sample Data

Sample data is automatically downloaded and installed by default. However, it's a reasonably large file and can take a while to download.

> "I don't want sample data"

Sample data installation can be disabled:

 * Open `Vagrantfile`
 * Change `sample_data = "true"` to `sample_data = "false"`
 * Run `vagrant up` as normal

> "I have already downloaded the sample data"

 * Place the sample data `tar.gz` file in the project root
 * Ensure `sample_data = "true"`
 * The provisioning script will skip the download and use the provided file instead. The same goes for when the provisioner is rerun. e.g. `vagrant reload --provision`

## Todo
* Install Modman.

**Why no Puppet/Chef?**
Admittedly, Puppet and Chef are excellent solutions for predictable and documented system configurations. The emphasis for this provisioner is on unopinionated simplicity. There are some excellent Puppet / Chef Magento configurations on Github with far more bells and whistles.
