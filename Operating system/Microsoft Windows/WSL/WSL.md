# WSL

## Các kiến thức cơ bản

- WSL chạy trên nền [[Hyper-V]]
- WSL hiện tại 2022-03-24 chưa hỗ trợ systemd
- 

### Cách tạo distro
1. Có thể tạo 1 distro WSL bất kỳ bằng cách archive 1 [[Docker]] container. Sau đó import vào WSL
2.  Cách làm cụ thể:
	- Run 1 docker image -> tạo ra container
	  
		```
		docker run -t centos bash ls /
		containerID=$(docker container ls -a| grep -i centos | awk '{print $1}')
		docker export $containerID > /mnt/d/Data/centos.tar
		```
		Việc trên sẽ tạo ra 1 snapshot của container đã tạo -> lưu thành file tar. Dùng file tar -> import vào WSL để có distro mới.
	- Dùng shell của windows thực hiện:
	  
		``` bash
		cd D:\Data
		mkdir D:\Data\CentOS # chứa data của WSL instance
		```
		
	- Import file đã tạo vào WSL

		``` powershell
		wsl --import CentOS D:\Data\CentOS .\centos.tar
		wsl -d CentOS
		```
		
	- Config WSL instance
	  
		``` bash
		yum update -y && yum install passwd sudo -y 
		myUsername=shiro 
		adduser -G wheel $myUsername 
		echo -e "[user]\ndefault=$myUsername" >> /etc/wsl.conf 
		passwd $myUsername
		```
		
		Then exit and restart.
		
		``` powershell
		wsl --terminate CentOS
		wsl -d CentOS
		```
		
#### Reference
	1. [Import any Linux distribution to use with WSL | Microsoft Docs](https://docs.microsoft.com/en-us/windows/wsl/use-custom-distro)