Using the DGX station
**************************

User Directories
=====================

Each user will have two directories according to the user's account::
 
 /home/user_name

and::

 /raid/home/user_name

Note that files under "/home/user_name" consume the 1.92 TB OS storage while files under "/raid/home/user_name" consume the 5.76 TB Data storage(see `here <http://dgx-wiki.readthedocs.io/en/latest/docs/environment/DGX.html#hardware-summary>`_ for detailed Hardware Summary of DGX station). 

To avoid the unnecessary consumption of OS storage, all users should only use::

 /raid/home/user_name

as their working directory.

Data Backup
=====================

Only storing data under "/raid" without a backup is unsafe since we use the `RAID 0 <https://en.wikipedia.org/wiki/Standard_RAID_levels#RAID_0>`_ configuration. 

All users are required to back up their data by themselves using their preferred ways under the directory:

 /data/dgx/backup/user_name

The data under "/data/dgx/backup" is stored in an external 8TB disk. It is also recommended that all users still make another copy of their important data outside the DGX station.