# Automatic creation of a NAT network and containers of Docker on Windows Server 2019
## Description of the problem
Microsoft has made an interesting surprise for all users in the work of Docker on Windows Server

> NAT networks created on Windows Server 2019 (or above) are no longer persisted after reboot.

If your container needs to receive network traffic, process it and then send it to another source, then you will have big problems after restarting the Windows Server 2019 host with containers

## Solving the problem
1. Install `Docker-compose` on Windows Server 2019
2. Creating a `docker-compose.yml` file with the configuration of the NAT network and all containers
3. We are writing a PowerShell script to run our docker-compose.yml file
4. In the `Windows Task Scheduler`, we create a task to run a PowerShell script on behalf of the `system user` when `At startup`

### Item 2. Example of a docker-compose.yml file
[DockerCompose-Windows-NAT-Java](/DockerCompose-Windows-NAT-Java/docker-compose.yml)

### Item 3. Example of a PowerShell script
	$f = "C:\docker-compose.yml"
	[string[]]$idContArr = docker.exe container ls -a -q
	[string[]]$idNetArr = docker.exe network ls -f 'driver=nat' -q
	
	if ($idContArr -gt 0)
	{
		docker.exe container stop $idContArr
	if ($idNetArr.Length -gt 0) 
	{ 
		docker.exe network rm $idNetArr 
	}
		docker.exe container rm $idContArr
	}
	
	docker-compose.exe -f $f up -d
	
### Disable default NAT
Additionally, you can disable the default NAT network in Docker in Windows Server 2019
Edit file `C:\ProgramData\docker\config` and add:

	{ 
		"bridge" : "none"
	}
