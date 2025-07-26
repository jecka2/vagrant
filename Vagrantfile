Vagrant.configure("2") do |config|
        config.vm.define "ubuntu" do |srv|
        srv.vm.box = "boxen/ubuntu-24.10"
        srv.vm.synced_folder "." "/home/jecka/Documents" , disabled: true
        srv.vm.hostname= "ubuntu"
        srv.vm.provider  "virtualbox" do |vb|
            vb.name = "ubuntu"
            vb.cpus = "1"
            vb.memory = "1024"
        end
    end
    config.vm.provision "ansible" do |ansible|
        ansible.playbook = "Vagrant.yml"
    end
end