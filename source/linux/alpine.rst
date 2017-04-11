Alpine Linux
============

Installation
------------

* Download the iso from alpine's website
* After bootup, login with root and no password
* Run the following commands to setup disk for lbu

.. code-block:: bash

   fdisk /dev/sda
   mkfs.vfat /dev/sda1
   mkdir /media/config
   echo "/dev/sda1 /media/config vfat rw 0 0" >> /etc/fstab
   mount -a
   setup-alpine
   lbu ci

.. note::
   For some reason the /dev/sda1 volume default is mounted not like defined in /etc/fstab.
   It is mounted read-only and on :code:`/media/sda1`.

Let's create a custom openrc script to fix that, remounting the /dev/sda1 partion to /media/config

.. code-block:: bash

   cat << EOF >> /etc/init.d/mount-config
   #!/sbin/openrc-run
   name="mount-config"
   command="umount /dev/sda1; mount -a"
   depend() {
     after sshd
   }
   EOF
   chmod +x /etc/init.d/mount-config
   rc-update add mount-config
   lbu add /etc/init.d/mount-config
   lbu commit

We can check if it is successful by

.. code-block:: bash

   df -h


