# CheckMK
Monitor all your Services and Apps

---
## Installation

### As [[Docker]]-Container
1. Run Docker.
   ```bash
   docker container run 
		   -dit
		   -p 8080:5000
		   -p 8000:8000
		   --tmpfs /opt/omd/sites/cmk/tmp:uid=1000,gid=1000
		   -v monitoring:/omd/sites
		   --name monitoring
		   -v /etc/localtime:/etc/localtime:ro
		   --restart always
	   checkmk/check-mk-raw:2.1.0-latest
   ```