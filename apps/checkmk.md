# CheckMK
Monitor all your Services and Apps

---
## Installation

### As [[Docker]]-Container

#### Raw-Edition
1. Run Docker.
	```bash
	   docker container run 
	   -dit
	   -p 8080:5000
	   -p 8000:8000
	   --tmpfs /opt/omd/sites/cmk/tmp:uid=1000,gid=1000
	   -v monitoring:/omd/sites
	   --name [name]
	   -v /etc/localtime:/etc/localtime:ro
	   --restart always
	   checkmk/check-mk-raw:2.1.0-latest
	```
   1. Navigate to [http://localhost:8080/cmk/check_mk/](http://localhost:8080/cmk/check_mk/) and Login with the Credentials.
	   2. The Default User is: `cmkadmin`
	   3. The Password is in the logs written for this Container (`docker container logs [name]`) 

#### Enterprise-Edition
1. Run Docker.
	1. Download the desired edition and version from the [Checkmk download page](https://checkmk.com/download?method=docker&edition=cfe&version=stable) (for the Free Edition) or from the [Checkmk customer portal](https://portal.checkmk.com/).
	2. Load the image from the downloaded tar archive file into Docker.
		   `docker load -i check-mk-enterprise-docker-2.1.0p5.tar.gz`
	1. You can then start the container with a very similar command to that described above.
	   ```bash
	   docker container run
	   -dit
	   -p 8080:5000
	   -p 8000:8000
	   --tmpfs /opt/omd/sites/cmk/tmp:uid=1000,gid=1000
	   -v monitoring:/omd/sites
	   --name [name]
	   -v /etc/localtime:/etc/localtime:ro
	   --restart always
	   checkmk/check-mk-enterprise:2.1.0p5
	   ```
	4. After starting the container, you can log in to the Checkmk web interface as described at the [Installation of the Raw Edition](#^a589e5).

### Bare Metal Installation

#### Debian/Ubuntu
1. Download the appropriate Package
	   If you have a subscription, in the Checkmk [customer portal](https://portal.checkmk.com/)  you will find a suitable RPM or DEB package for your distribution for every available [Checkmk](https://docs.checkmk.com/latest/en/cmk_versions.html) version. For a free test or smaller installations of Checkmk you can use our [free editions](https://checkmk.com/de/download).
2. Signed-package installation.
	1. Depending on which optional packages were installed during the setup of your distribution, you will need to install `dpkg-sig` including all of its dependencies for successful verification.
		   `apt install dpkg-sig`
	2. Download the public key from checkmk
		   `wget https://download.checkmk.com/checkmk/Check_MK-pubkey.gpg`
	3. Import the key
		   `gpg --import Check_MK-pubkey.gpg`
	4. Verify the Checkmk package
		   `dpkg-sig --verify check-mk-raw-[version].focal_amd64.deb`
	5. Install the Checkmk package
		   `apt install [/path/to/.deb-file]`
3. Final test
	   After the successful installation of Checkmk, and all dependencies, you will have access to the omd command. With this command you are able to create and manage monitoring sites. You can request the installed version to verify your installation. `omd version`

---
## Backup/Restore for Sites

### Prerequisites
>[!warning]
>In order to create backups on the source system ater 'restore' them on a target system, two main requirements must be met:
>1. The exact same Checkmk version must be installed on both systems, e.g. `2.1.0p1`.
>2. Both systems must have the same Checkmk edition installed, e.g. `Checkmk Raw Edition`

>[!o-note] Note
>The system platform is not relevant

### Backup/Restore with OMD

#### Creating a Backup
***Note**: Checkmk also offers the possibility of performing backups and restores without using the command line, via the [GUI](#^78d8ec).*

##### 1. To/From File
1. Backup
	1. as Site-User
		   `omd backup [/path/to/file.tar.gz]`
	2. as Root
		   `omd backup [sitename] [/path/to/file.tar.gz]`
2. Restore
	1. as Site-User
		   `omd restore [path/to/file.tar.gz]`
	2. as Root
		   `omd restore [sitename] [path/to/file.tar.gz]`
##### 2. Direct over SSH
1. Source System -> Backup System
	   `omd backup ([sitename]) - | ssh user@otherserver "omd restore -"`
2. Backup System <- Source System
	   `ssh mysite@checkmkserver "omd backup ([sitename]) -" | omd restore -`


>[!o-info] Info
>>[!warning] Warning
>>if on the backup system already a site with this name you have to delete it by adding these argument: `--reuse --kill`
>#
>>[!o-note] Restore with different Name
>>You can also restore a site under another name. Simply restore the Backup as root and enter the new site name. Checkmk do the rest for you.
>#
>>[!note] Backup without History
>>You can also create a Backup without history, simply add following argument: `-N`



### Backup/Restore with GUI
#### 1. Configuring encrypted backup
1. Go to backup targets on the *Site backup page* and click on *Add target*
2. Enter a unique ID, Title and absolute fie path. 
   >***Note:** The file path refers to the system, not the site.*
3. Then go back and click on *Backup encryption keys* and create a new key on *Add key*
4. Specify a meaningful name and a secure password unter *Passphrase*
   >***Note:** You can download the key in PEM format because you need these keys to restore backups.*

#### 2. Creating a backup job
1. Now you can create a new backup job - again on the *Site backup* page on the *Add job* button
2. Here you can select among other things. The Items you just created under *Target* and *Encryption*. You will also find options for compression and scheduling backups. Enter all the options to your needs.

#### 2. Creating a backup
1. On the *Site backup* page you should now see your finished backup job and start using ![[icon_backup_start.png]]

#### 3. Restore backup
You can restore backups by clicking on Restore on the *Site backup* page
1. Select the backup target with ![[icon_backup_restore.png]] 
2. Select the backup to he restored with ![[icon_backup_restore.png]]
3. Enter the passphrase for the backup key
4. Start the restore.
>***Note:** After restoring the site will be restarted, so you will temporarily see an HTTP 503 error message.*