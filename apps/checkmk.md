# CheckMK
Monitor all your Services and Apps

---
## Installation

### As [[Docker]]-Container
#### Raw-Version
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
   2. Navigate to [http://localhost:8080/cmk/check_mk/](http://localhost:8080/cmk/check_mk/) and Login with the Credentials.
	   2. The Default User is: `cmkadmin`
	   3. The Password is in the logs written for this Container (`docker container logs [name]`)

#### Enterprise-Version
1. Run Docker.
	1. Download the desired edition and version from the [Checkmk download page](https://checkmk.com/download?method=docker&edition=cfe&version=stable) (for the Free Edition) or from the [Checkmk customer portal](https://portal.checkmk.com/).
	2. Load the image from the downloaded tar archive file into Docker.
	   `docker load -i check-mk-enterprise-docker-2.1.0p5.tar.gz`
	3. You can then start the container with a very similar command to that described above.
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
	4. After starting the container, you can log in to the Checkmk web interface as described at the [test]([[#Raw-Version]])