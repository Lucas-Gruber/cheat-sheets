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
   ``` ^ad76d6
   2. Navigate to [http://localhost:8080/cmk/check_mk/](http://localhost:8080/cmk/check_mk/) and Login with the Credentials.
	   2. The Default User is: `cmkadmin`
	   3. The Password is in the logs written for this Container (`docker container logs [name]`) ^a589e5

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
In order to create backups on the source system and later 'restore' them on a target system, two main requirements must be met:
1. The exact same Checkmk version must be installed on both systems, e.g. `2.1.0p1`.
2. Both systems must have the same Checkmk edition installed, e.g. the **Checkmk Raw Edition**

*Note: the system platform is not relevant*

### Backup/Restore with OMD

#### Creating a Backup
***Note**: Checkmk also offers the possibility of performing backups and restores without using the command line, via the [GUI](#^78d8ec).*
1. Backing up a site with `omd backup` does 
2. 

### Backup/Restore with GUI
tst
test