
## Задание 18.Vagrant

### Обновить ядро в базовой системе

### Цель:
Получить навыки работы с Git, Vagrant;
Обновлять ядро в ОС Linux.


### Решение:

Используя готовый бокс Vagrant  boxen/ubuntu-24.10  проводим  настройку виртуальнгого окружения с помощью  Vagrantfile 
```bash 
srv.vm.provider  "virtualbox" do |vb|
            vb.name = "ubuntu"
            vb.cpus = "1"
            vb.memory = "1024"
```

Указваем имя машины ubuntu, систему виртуализации virtualbox и количество ресурсов ( cpu 1, Memory  1024  )
Далее указываем, что мы будем использовать в качестве provision  системы  ansible  и просим выполнить его playbook Vagrant.yml

Данный плейбук выполнит следующие действия:
1) Укажет текущее использыемое системой ядро (kernel)
2) Произведет скачивание ядра системы для Ubuntu 6.14.10
3) Произведет обновение Кэша репозиториев
4) Произведет установку ядра 6.14.10
5) Произведет перезагрузку системы и после перезагрузки подключится снова
6) Произведет обнлвение данных о машине и выведет информацию о текущем используемомо ядре (kernel)

### Отчет о выполнении
```bash
Bringing machine 'ubuntu' up with 'virtualbox' provider...
==> ubuntu: Importing base box 'boxen/ubuntu-24.10'...
==> ubuntu: Matching MAC address for NAT networking...
==> ubuntu: Checking if box 'boxen/ubuntu-24.10' version '2025.04.02.21' is up to date...
==> ubuntu: Setting the name of the VM: ubuntu
==> ubuntu: Clearing any previously set network interfaces...
==> ubuntu: Preparing network interfaces based on configuration...
    ubuntu: Adapter 1: nat
==> ubuntu: Forwarding ports...
    ubuntu: 22 (guest) => 2222 (host) (adapter 1)
==> ubuntu: Running 'pre-boot' VM customizations...
==> ubuntu: Booting VM...
==> ubuntu: Waiting for machine to boot. This may take a few minutes...
    ubuntu: SSH address: 127.0.0.1:2222
    ubuntu: SSH username: vagrant
    ubuntu: SSH auth method: private key
    ubuntu: 
    ubuntu: Vagrant insecure key detected. Vagrant will automatically replace
    ubuntu: this with a newly generated keypair for better security.
    ubuntu: 
    ubuntu: Inserting generated public key within guest...
    ubuntu: Removing insecure key from the guest if it's present...
    ubuntu: Key inserted! Disconnecting and reconnecting using new SSH key...
==> ubuntu: Machine booted and ready!
==> ubuntu: Checking for guest additions in VM...
    ubuntu: The guest additions on this VM do not match the installed version of
    ubuntu: VirtualBox! In most cases this is fine, but in rare cases it can
    ubuntu: prevent things such as shared folders from working properly. If you see
    ubuntu: shared folder errors, please make sure the guest additions within the
    ubuntu: virtual machine match the version of VirtualBox you have installed on
    ubuntu: your host and reload your VM.
    ubuntu: 
    ubuntu: Guest Additions Version: 7.0.20
    ubuntu: VirtualBox Version: 7.1
==> ubuntu: Setting hostname...
==> ubuntu: Mounting shared folders...
    ubuntu: /home/jecka/Documents/otus/vagrant => /vagrant
==> ubuntu: Running provisioner: ansible...
    ubuntu: Running ansible-playbook...

PLAY [Update kernel] ***********************************************************

TASK [Gathering Facts] *********************************************************
[WARNING]: Platform linux on host ubuntu is using the discovered Python
interpreter at /usr/bin/python3.12, but future installation of another Python
interpreter could change the meaning of that path. See
https://docs.ansible.com/ansible-
core/2.18/reference_appendices/interpreter_discovery.html for more information.
ok: [ubuntu]

TASK [Display kernel version] **************************************************
ok: [ubuntu] => {
    "msg": "The Linux kernel version on ubuntu is 6.11.0-21-generic"
}

TASK [Apt download] ************************************************************
changed: [ubuntu] => (item={'filename': 'linux-headers-6.14.10-061410-generic_6.14.10-061410.202506041434_amd64.deb', 'destdir': '/tmp'})
changed: [ubuntu] => (item={'filename': 'linux-headers-6.14.10-061410_6.14.10-061410.202506041434_all.deb', 'destdir': '/tmp'})
changed: [ubuntu] => (item={'filename': 'linux-image-unsigned-6.14.10-061410-generic_6.14.10-061410.202506041434_amd64.deb', 'destdir': '/tmp'})
changed: [ubuntu] => (item={'filename': 'linux-modules-6.14.10-061410-generic_6.14.10-061410.202506041434_amd64.deb', 'destdir': '/tmp'})

TASK [Ensure Apt is up to date] ************************************************
changed: [ubuntu]

TASK [Apt install] *************************************************************
changed: [ubuntu] => (item=/tmp/linux-headers-6.14.10-061410_6.14.10-061410.202506041434_all.deb)
changed: [ubuntu] => (item=/tmp/linux-modules-6.14.10-061410-generic_6.14.10-061410.202506041434_amd64.deb)
changed: [ubuntu] => (item=/tmp/linux-headers-6.14.10-061410-generic_6.14.10-061410.202506041434_amd64.deb)
changed: [ubuntu] => (item=/tmp/linux-image-unsigned-6.14.10-061410-generic_6.14.10-061410.202506041434_amd64.deb)

TASK [Reboot the machine and wait for it to come back online] ******************
changed: [ubuntu]

TASK [Re-gather facts after package installation] ******************************
ok: [ubuntu]

TASK [Display kernel version] **************************************************
ok: [ubuntu] => {
    "msg": "The Linux kernel version on ubuntu is 6.14.10-061410-generic"
}

PLAY RECAP *********************************************************************
ubuntu                     : ok=8    changed=4    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
```