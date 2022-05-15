# Vagrant

## Why Vagrant ?

1. Vagrant là 1 công cụ quản lý các máy ảo

#### Vagrant provisioning
1. Add ssh key có sẵn: Nếu đã có cặp key-pair, chỉ cần add content của public key vào máy ảo.
	- Code thêm SSH key
	  
	``` ruby
	config.vm.provision "shell" do |s|
		ssh_pub_key = File.readlines("{{public_file_path}}").first.strip
		s.inline = <<-SHELL
			sudo mkdir /root/.ssh
			sudo chmod 600 ~/.ssh
			sudo touch ~/.ssh/authorized_keys
			sudo chmod 600 ~/.ssh/authorized_keys
			sudo echo #{ssh_pub_key} >> /root/.ssh/authorized_keys
		SHELL
	end	
	```