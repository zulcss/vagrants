Vagrant Development Boxes
=========================

Preliminaries: Install Vagrant(Ubuntu)
--------------------------------------
[Vagrant][vagrant] is a tool to "create and configure lightweight, reproducible,
and portable development environments." Vagrant itself is a virtual instance
creation and startup tool on top of Oracle VirtualBox which takes care of the
virtualisation.

Install the Open Source Edition of VirtualBox and Vagrant itself:

    sudo apt-get install virtualbox-ose
    wget http://files.vagrantup.com/packages/eb590aa3d936ac71cbf9c64cf207f148ddfc000a/vagrant_1.0.3_x86_64.deb
    sudo dpkg -i vagrant_1.0.3_x86_64.deb


Preliminaries: IntelliJ Project Files
-------------------------------------
Because this is not a builded project, there is no support for automatically
creating IntelliJ project definitions. If you want to use the IntelliJ editors
you will need to create a new project in IntelliJ. Use the Create project from
existing sources, unmark the source files that IntelliJ finds and confirm
project creation.

In the new project, go to Project Structure and create a new module from scratch
specifying the project base directory as the content root. Do not create a
source directory and finish. In the newly constructed module, add the project
base directory as a sources directory.

Your project should now be setup for editing.


frontend
--------
Vagrant box supporting Guardian [frontend][frontend].

    /opt/vagrant/bin/vagrant init frontend_lucid64 http://path-to-frontend_lucid64.box
    /opt/vagrant/bin/vagrant up

And ssh onto it:

    /opt/vagrant/bin/vagrant ssh

To build the package from scratch:

    cd frontend_lucid64
    /opt/vagrant/bin/vagrant up
    /opt/vagrant/bin/vagrant package
    mv package.box frontend_lucid64.box

The puppet provisioning during the `vagrant up` step may take some time.

Port 80 is forwarded to port 8000 on the host machine. See:

    http://localhost:8000



frontend_extras
---------------
A version of `frontend` with ElasticSearch and Mongodb.

    /opt/vagrant/bin/vagrant init frontend_extras_lucid64 http://path-to-frontend_extras_lucid64.box
    /opt/vagrant/bin/vagrant up

And ssh onto it:

    /opt/vagrant/bin/vagrant ssh

To build the package from scratch, first build `frontend`, then:

    cd frontend_extras_lucid64
    /opt/vagrant/bin/vagrant up
    /opt/vagrant/bin/vagrant package
    mv package.box frontend_extras_lucid64.box

The puppet provisioning during the `vagrant up` step may take some time.

Port 80 is forwarded to port 8000 on the host machine. See:

    http://localhost:8000

Port 9200 is forwarded, so the ElasticSearch server is available from the host
machine. In addition, a number of plugins have been included. See:

    http://localhost:9200/_plugin/head/
    http://localhost:9200/_plugin/paramedic/index.html
    http://localhost:9200/_plugin/bigdesk/

Ports 27017 and 28017 are forwarded to the host machine for MongoDB. See:

    http://localhost:28017


Vagrant Commmands
-----------------

* `/opt/vagrant/bin/vagrant suspend`: Disable the virtual instance. The
  allocated disc space for the instance is retained but the instance will not be
  available. The running state at suspend time is saved for resumption.
* `/opt/vagrant/bin/vagrant resume`: Wake up a previously suspended virtual
  instance.
* `/opt/vagrant/bin/vagrant halt`: Turn off the virtual instance. Calling
  `vagrant up` after this is the equivalent of a reboot.
* `/opt/vagrant/bin/vagrant destroy`: Hose your virtual instance, reclaiming the
  allocated disc space.
* `/opt/vagrant/bin/vagrant provision`: Rerun puppet or chef provisioning on the
  virtual instance.


Vagrant Troubleshooting
-----------------------

To see more verbose output on any vagrant command, add a VAGRANT_LOG environment
variable setting, e.g.:

    VAGRANT_LOG=INFO /opt/vagrant/bin/vagrant up

Further help troubleshooting can be obtained by editing your `Vagrantfile` and
enabling the `config.vm.boot_mode = :gui` setting. This will pop up a VirtualBox
GUI window on boot.

There have been some issues getting 64bit instances to start. The error is
apparent in GUI boot:

    VT-x/AMD-V hardware acceleration has been enabled, but is not
operational. Your 64-bit guest will fail to detect a 64-bit CPU and
will not be able to boot.

Some BIOS setting changes can help. The changes are described at
`http://dba-star.blogspot.com/2011/11/how-to-enable-vtx-and-vtd-on-hp-compaq.html`
but briefly:

1. Restart your developer box.
2. Press F10 for BIOS settings at the boot splash.
3. Edit Security -> System Security (I wasn't expecting it here either!)
4. Enable VT-x and VT-d settings.
5. Save and exit.



[frontend]: https://github.com/guardian/frontend
[vagrant]: http://vagrantup.com
