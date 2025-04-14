# VAGRANTFILE EXAMPLES CHEATSHEET

#==========================================
# BASIC CONFIGURATION
#==========================================

# Minimal Vagrantfile
Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/focal64"
end

# Specific version of a box
Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/focal64"
  config.vm.box_version = "20220804.0.0"
end

# Basic VM customization
Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/focal64"
  config.vm.hostname = "devserver"
  config.vm.provider "virtualbox" do |vb|
    vb.memory = 4096
    vb.cpus = 2
    vb.name = "development-vm"
  end
end

#==========================================
# NETWORKING EXAMPLES
#==========================================

# Port forwarding
Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/focal64"
  # Forward guest port 80 to host port 8080
  config.vm.network "forwarded_port", guest: 80, host: 8080
  # Forward guest port 3306 to host port 3306
  config.vm.network "forwarded_port", guest: 3306, host: 3306
end

# Private network (static IP)
Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/focal64"
  config.vm.network "private_network", ip: "192.168.33.10"
end

# Private network (DHCP)
Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/focal64"
  config.vm.network "private_network", type: "dhcp"
end

# Public network (bridged)
Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/focal64"
  config.vm.network "public_network"
  # Optionally specify bridge interface
  # config.vm.network "public_network", bridge: "en0: Wi-Fi (AirPort)"
end

#==========================================
# FOLDER SYNCING
#==========================================

# Basic shared folder
Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/focal64"
  # Share current directory to /vagrant
  config.vm.synced_folder ".", "/vagrant"
  # Share specific host path to VM path
  config.vm.synced_folder "../data", "/vm-data"
end

# Advanced shared folder options
Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/focal64"
  # Disabled default sync
  config.vm.synced_folder ".", "/vagrant", disabled: true
  # NFS for better performance (Unix hosts only)
  config.vm.synced_folder "./src", "/var/www", 
    type: "nfs", 
    mount_options: ["dmode=775,fmode=664"]
  # SMB for Windows hosts
  config.vm.synced_folder "./src", "/var/www", type: "smb"
  # Rsync synced folder
  config.vm.synced_folder "./src", "/var/www", 
    type: "rsync",
    rsync__exclude: [".git/", "node_modules/"]
end

#==========================================
# PROVISIONING
#==========================================

# Shell provisioning
Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/focal64"
  # Inline shell script
  config.vm.provision "shell", inline: <<-SHELL
    apt-get update
    apt-get install -y nginx
    systemctl enable nginx
    systemctl start nginx
  SHELL
  
  # External shell script
  config.vm.provision "shell", path: "bootstrap.sh"
  
  # With args
  config.vm.provision "shell", path: "script.sh", args: ["arg1", "arg2"]
  
  # Run as a specific user
  config.vm.provision "shell", inline: "echo 'Hello'", 
    privileged: false  # Run as vagrant user instead of root
end

# File provisioning
Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/focal64"
  # Upload a file
  config.vm.provision "file", source: "~/files/config.json", destination: "~/config.json"
  # Upload a folder
  config.vm.provision "file", source: "~/configs", destination: "~/configs"
end

# Ansible provisioning
Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/focal64"
  # Basic playbook
  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "playbook.yml"
  end
  
  # Advanced options
  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "playbook.yml"
    ansible.inventory_path = "inventory"
    ansible.limit = "all"
    ansible.extra_vars = { 
      mysql_password: "secret" 
    }
    ansible.verbose = "v"
  end
end

# Docker provisioning
Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/focal64"
  config.vm.provision "docker" do |d|
    d.run "nginx", 
      args: "-p 80:80 -v /vagrant/nginx:/usr/share/nginx/html"
    d.run "mysql", 
      args: "-e MYSQL_ROOT_PASSWORD=root -p 3306:3306"
  end
end

#==========================================
# MULTI-MACHINE SETUP
#==========================================

# Basic multi-machine setup
Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/focal64"

  # Web server VM
  config.vm.define "web" do |web|
    web.vm.hostname = "web"
    web.vm.network "private_network", ip: "192.168.33.10"
    web.vm.provider "virtualbox" do |vb|
      vb.memory = 1024
      vb.cpus = 1
    end
    web.vm.provision "shell", inline: "apt-get update && apt-get install -y nginx"
  end

  # Database VM
  config.vm.define "db" do |db|
    db.vm.hostname = "db"
    db.vm.network "private_network", ip: "192.168.33.11"
    db.vm.provider "virtualbox" do |vb|
      vb.memory = 2048
      vb.cpus = 2
    end
    db.vm.provision "shell", inline: "apt-get update && apt-get install -y mysql-server"
  end
end

# Advanced multi-machine with primary machine
Vagrant.configure("2") do |config|
  
  # Common configuration
  config.vm.box = "ubuntu/focal64"
  config.vm.provision "shell", inline: "apt-get update"
  
  # Load balancer
  config.vm.define "lb" do |lb|
    lb.vm.hostname = "lb"
    lb.vm.network "private_network", ip: "192.168.33.10"
    lb.vm.provision "shell", inline: "apt-get install -y haproxy"
  end
  
  # Web servers
  (1..3).each do |i|
    config.vm.define "web#{i}" do |web|
      web.vm.hostname = "web#{i}"
      web.vm.network "private_network", ip: "192.168.33.#{10+i}"
      web.vm.provision "shell", inline: "apt-get install -y apache2"
    end
  end
  
  # Database server - set as primary
  config.vm.define "db", primary: true do |db|
    db.vm.hostname = "db"
    db.vm.network "private_network", ip: "192.168.33.20"
    db.vm.provision "shell", inline: "apt-get install -y mysql-server"
  end
end

#==========================================
# DISK MANAGEMENT
#==========================================

# Add a 32GB disk with VirtualBox provider
Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/focal64"
  
  config.vm.provider "virtualbox" do |vb|
    # Create additional disk if it doesn't exist
    unless File.exist?('./second_disk.vdi')
      vb.customize ['createhd', 
                    '--filename', './second_disk.vdi', 
                    '--variant', 'Fixed', 
                    '--size', 32 * 1024]
    end
    
    # Attach the disk to the VM
    vb.customize ['storageattach', :id, 
                  '--storagectl', 'SATA Controller', 
                  '--port', 1, 
                  '--device', 0, 
                  '--type', 'hdd', 
                  '--medium', './second_disk.vdi']
  end
  
  # Configure disk as LVM inside VM
  config.vm.provision "shell", inline: <<-SHELL
    # Install LVM tools
    apt-get update
    apt-get install -y lvm2
    
    # Wait for the disk to be recognized
    sleep 5
    
    # Find the name of the new disk (usually sdb but could be different)
    NEW_DISK=$(lsblk -d -p -n -l -o NAME | grep -v sda | head -n 1)
    
    # Setup LVM if disk doesn't already have a partition
    if [ -z "$(blkid $NEW_DISK)" ]; then
      # Create physical volume
      pvcreate $NEW_DISK
      
      # Create volume group
      vgcreate vg_data $NEW_DISK
      
      # Create logical volume using all space
      lvcreate -l 100%FREE -n lv_data vg_data
      
      # Format with ext4
      mkfs.ext4 /dev/vg_data/lv_data
      
      # Create mount point
      mkdir -p /mnt/data
      
      # Add to fstab for persistent mount
      echo '/dev/vg_data/lv_data /mnt/data ext4 defaults 0 0' >> /etc/fstab
      
      # Mount the volume
      mount /mnt/data
      
      # Set permissions
      chmod 777 /mnt/data
    fi
  SHELL
end

# Add a disk with VMware provider
Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/focal64"
  
  config.vm.provider "vmware_desktop" do |vmware|
    vmware.vmx["scsi0:1.present"] = "TRUE"
    vmware.vmx["scsi0:1.fileName"] = "extra_disk.vmdk"
    vmware.vmx["scsi0:1.fileSize"] = 32 * 1024  # Size in MB
  end
  
  # Same LVM provisioning script as above
end

#==========================================
# PROVIDER-SPECIFIC CONFIGURATIONS
#==========================================

# VirtualBox specific settings
Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/focal64"
  
  config.vm.provider "virtualbox" do |vb|
    # VM name in VirtualBox
    vb.name = "my_project_vm"
    
    # Hardware settings
    vb.memory = 4096
    vb.cpus = 2
    
    # Graphics settings
    vb.gui = true
    vb.customize ["modifyvm", :id, "--vram", "128"]
    
    # Performance settings
    vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
    vb.customize ["modifyvm", :id, "--natdnsproxy1", "on"]
    vb.customize ["modifyvm", :id, "--ioapic", "on"]
    
    # Clipboard and drag-and-drop
    vb.customize ["modifyvm", :id, "--clipboard", "bidirectional"]
    vb.customize ["modifyvm", :id, "--draganddrop", "bidirectional"]
  end
end

# VMware specific settings
Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/focal64"
  
  config.vm.provider "vmware_desktop" do |vmware|
    vmware.gui = true
    vmware.vmx["memsize"] = "4096"
    vmware.vmx["numvcpus"] = "2"
    vmware.vmx["ethernet0.virtualDev"] = "vmxnet3"
    vmware.vmx["ethernet0.present"] = "TRUE"
    vmware.vmx["sound.present"] = "TRUE"
    vmware.vmx["sound.autodetect"] = "TRUE"
  end
end

# Hyper-V specific settings
Vagrant.configure("2") do |config|
  config.vm.box = "generic/ubuntu2004"
  
  config.vm.provider "hyperv" do |h|
    h.memory = 4096
    h.cpus = 2
    h.enable_virtualization_extensions = true
    h.linked_clone = true
  end
end

#==========================================
# PLUGIN CONFIGURATIONS
#==========================================

# Vagrant Cachier for package caching
Vagrant.configure("2") do |config|
  if Vagrant.has_plugin?("vagrant-cachier")
    config.cache.scope = :box
    config.cache.synced_folder_opts = {
      type: :nfs,
      mount_options: ['rw', 'vers=3', 'tcp', 'nolock']
    }
  end
end

# VBGuest auto-update
Vagrant.configure("2") do |config|
  if Vagrant.has_plugin?("vagrant-vbguest")
    config.vbguest.auto_update = true
    config.vbguest.no_remote = false
  end
end

#==========================================
# ADVANCED TECHNIQUES
#==========================================

# Environment variables in Vagrantfile
Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/focal64"
  
  # Use environment variables with default fallbacks
  memory = ENV['VM_MEMORY'] || "4096"
  cpus = ENV['VM_CPUS'] || "2"
  
  config.vm.provider "virtualbox" do |vb|
    vb.memory = memory
    vb.cpus = cpus
  end
end

# Conditional configuration
Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/focal64"
  
  # Detect host OS and apply different configurations
  case RUBY_PLATFORM
  when /darwin/
    # macOS specific settings
    config.vm.synced_folder ".", "/vagrant", type: "nfs"
  when /mswin|mingw|cygwin/
    # Windows specific settings
    config.vm.synced_folder ".", "/vagrant", type: "smb"
  when /linux/
    # Linux specific settings
    config.vm.synced_folder ".", "/vagrant", type: "nfs"
  end
end

# Custom triggers
Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/focal64"
  
  # Before starting VM
  config.trigger.before :up do |trigger|
    trigger.info = "Running pre-up script..."
    trigger.run = {path: "scripts/pre-up.sh"}
  end
  
  # After VM is started
  config.trigger.after :up do |trigger|
    trigger.info = "Running post-up script..."
    trigger.run = {path: "scripts/post-up.sh"}
  end
  
  # Before destroying VM
  config.trigger.before :destroy do |trigger|
    trigger.info = "Backing up data..."
    trigger.run_remote = {inline: "mkdir -p /tmp/backup && cp -R /var/www /tmp/backup"}
    trigger.run = {inline: "mkdir -p backups && vagrant scp default:/tmp/backup backups/"}
  end
end 