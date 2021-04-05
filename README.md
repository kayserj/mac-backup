
# Mac Backup Ansible Playbook  

This playbook is a collection of tasks that will perform daily, and weekly backups, as well as monthly archiving of certain cloud file services on your MacOS device.

## History  
I was recently made aware of "World Backup Day", so I figured I would share the Ansible playbook I use to back up my data, as well as the strategy/reasoning behind why.  

My data is stored in 2 cloud services platforms.  Dropbox, and iCloud (iCloud data drive, and iCloud photo app).  This is the data that I will use on a daily basis.  Adds, moves and changes made to any file within these services will be synchronized/replicated to the cloud, and other participating devices, in near real-time (give or take 10 minutes, but you get the idea).  The more participating devices you have with a full copy of your data, the more redundant, and resilient you will be.  I use this method because it meets several good backup practice criteria. 

## Data Usage and Backup Criteria

1.  Data should be onsite, and offsite.  
2.  Data should be on multiple disks.  
3.  Data should be synchronized.  
4.  Data should be backed up, and archived.

Cloud Services enable my data to be offsite, synchronized, and on redundant disk arrays.  Synchronizing that data onto my laptop, or another participating device provides the onsite criteria, and more multiple disk redundancy. 

Despite the restore functionality of the cloud services, it is not a good idea to rely solely upon them for recovery or file change control.  Failure of any participating cloud service device, or the cloud service itself, cannot create a situation of irrecoverable data, however it is not outside the realm of possibility for all to go down at once, no matter how unlikely.  There is also the possibility of file deletion propagation.  Files that get deleted cause a ripple effect of file deletions across all synchronized devices, to include the cloud.  Hopefully your cloud service is intact for you to recover. If not, go to the backup!!


![Image of Backup Plan](https://github.com/kayserj/project-images/blob/master/mac-backup.png)


## Backup Plan
To put it short, we need to backup all this synchronized data in another location, just in case it all disappears.  This location can be an external hard drive, a share on a NAS, or even an iSCSI target.  As long as your location is a mountable volume, you should be fine.  

1.  Daily Backup - A copy of today's data.
2.  Weekly Backup - A copy of the data on Friday.
3.  Monthly Backup - A copy of the data on the first of the month, then compressed to archive (.zip file)


##  How to Install and Execute  

Download the playbook to your MacOS device that is also running ansible, from within the folder type the following to execute the playbook

```
ansible-playbook backup-mac.yml
```

Once the playbook is executed, it will make sure the destination volume path is valid, then proceed to back up your Dropbox and iCloud data, to the volume specified in the vars section.  It assumes Dropbox and iCloud are installed in their default locations.  If you have not installed Dropbox or iCloud to their default locations, you will need to specify the location in the vars section of the playbook. 

### Details
The playbook uses the synchronize module to provide rsync capabilities to the daily and weekly backup, and the archive module to compress the data monthly.  As-is, you have to run the playbook manually.  To automate the process, use a cronjob.  

```
crontab -e

30 18 * * * ansible-playbook backup-mac.yml
```

### Built With

* [Ansible](https://www.ansible.com/)


### Authors

**James Kayser** - *Initial work* - [kayserj](https://github.com/kayserj)

### Special Thanks. 
Mark Sanborn.  
https://www.marksanborn.net/howto/use-rsync-for-daily-weekly-and-full-monthly-backups/  

and   


World Backup Day.  
http://www.worldbackupday.com/en/


