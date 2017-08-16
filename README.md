This vagrant template is prepared to install a fresh new instance of iTop 2.3.4

The script will install all its dependencies and default all passwords

Default users and passwords:
-> DB
--> User: root
---> Password: MySuperPassword
--> User: itop-user
---> Password: MyPassword
-> iTop
--> User: admin
---> Password: admin

The script is prepared to execute an automated iTop installation without any
interaction, but if you prefer to execute the installation of iTop manually
comment line "sudo -u www-data php unattended_install.php default-params.xml"
on Vagrantfile.

Also, sample data is not included by default, if the system needs to be filled
with demo data modify line "<sample_data>0</sample_data>" to 1 before starting
the machine the first time.

More information on automated iTop installations at:
https://wiki.openitop.org/doku.php?id=2_3_0:advancedtopics:automatic_install

In case of any issues please let me know so I can maintain and improve the
script.

This is only inteded to create instances for test or demo purposes.