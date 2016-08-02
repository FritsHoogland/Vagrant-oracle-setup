# -*- mode: ruby -*-
Vagrant.configure("2") do |config|
  config.vm.box = "boxcutter/ol72"
  # synced_folder is disabled. it requires a kernel module that is specific to a kernel.
  # this module will not be there once the kernel is upgraded.
  config.vm.synced_folder ".", "/vagrant", disabled: true
  # ssh settings
  config.ssh.insert_key = false
  config.ssh.private_key_path = [ "~/.vagrant_ssh/id_rsa", "~/.vagrant.d/insecure_private_key" ]
  config.vm.provision "file", source: "~/.vagrant_ssh/id_rsa.pub", destination: "~/.ssh/authorized_keys"
  # virtual machine hostname as set by linux.
  config.vm.hostname = "ol72-oraclepub.local"
  # virtual machine ip address, set this to match your environment.
  config.vm.network "private_network", ip: ""

  config.vm.provider "virtualbox" do |vb|
   # memory and cpus. set this according to your needs and machine resources.
   vb.memory = 3072
   vb.cpus = 3

   # u01
   u01_disk = "u01_disk.vdi"
   if !File.exist?(u01_disk)
     # 40960 = 40G
     vb.customize [ 'createhd', '--filename', u01_disk, '--size', 40960 ]
   end
   vb.customize [ 'storageattach', :id, '--storagectl', 'SATA Controller', '--port', 2, '--device', 0, '--type', 'hdd', '--medium', u01_disk ]
   # data
   data_disk = "data_disk.vdi"
   if !File.exist?(data_disk)
     # 40960 = 40G
     vb.customize [ 'createhd', '--filename', data_disk, '--size', 40960 ]
   end
   vb.customize [ 'storageattach', :id, '--storagectl', 'SATA Controller', '--port', 3, '--device', 0, '--type', 'hdd', '--medium', data_disk ]
 
  end
 
  # ansible settings.
  config.vm.provision "ansible" do |ansible|
    ansible.extra_vars = {
      mosuser: "",
      mospass: "",
      host_set_ip_address: "",
      oracle_home: "/u01/app/oracle/product/12.1.0.2/dbhome_1",
      oracle_base: "/u01/app/oracle",
      database_name: "testdb",
      global_password: "oracle",
      asm_create_file_dest: "DATA",
      db_create_file_dest: "/u01/app/oracle/oradata"
    }
    ansible.playbook = "ansible/12102_filesystem.yml"
  end

end
