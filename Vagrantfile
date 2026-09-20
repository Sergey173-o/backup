Vagrant.configure("2") do |config|

  config.vm.define "backup_server" do |bk|
    bk.vm.box = "generic/ubuntu2204"
    bk.vm.boot_timeout = 600
    bk.vm.hostname = "backup-server"

    bk.vm.provider "virtualbox" do |vb|
      vb.memory = 1024
      vb.cpus = 1
      vb.name = "backup-server-vm"
    end

    bk.vm.network "private_network", ip: "192.168.56.11"

    # Отдельный диск 2GB под /var/backup (монтирует и форматирует Ansible)
    bk.vm.disk :disk, size: "2GB", name: "backup_disk"
  end

  config.vm.define "client" do |cl|
    cl.vm.box = "generic/ubuntu2204"
    cl.vm.boot_timeout = 600
    cl.vm.hostname = "client"

    cl.vm.provider "virtualbox" do |vb|
      vb.memory = 1024
      vb.cpus = 1
      vb.name = "client-vm"
      vb.gui = true
    end

    cl.vm.network "private_network", ip: "192.168.56.12"

    # Ansible-провижининг запускаем один раз, с последней машины, на все хосты сразу
    cl.vm.provision "ansible" do |ansible|
      ansible.playbook = "ansible/playbook.yml"
      ansible.limit = "all"
      ansible.groups = {
        "backup_server" => ["backup_server"],
        "client"        => ["client"]
      }
    end
  end

end
