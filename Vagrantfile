# -*- mode: ruby -*-
# vi: set ft=ruby :

DOMAIN='katello.example.com'
SUBNET="172.16.5"

Vagrant.configure("2") do |config|

  config.hostmanager.enabled = true
  config.hostmanager.manage_host = false
  config.hostmanager.ignore_private_ip = false
  config.hostmanager.include_offline = true

  config.vm.provision :hostmanager

  config.vm.box = "centos65-x86_64-20140116"
  config.vm.box_url = "https://github.com/2creatives/vagrant-centos/releases/download/v6.5.3/centos65-x86_64-20140116.box"

  config.vm.provision "shell", 
    inline: "/etc/init.d/iptables stop"

  config.vm.define :katello do |katello|
    katello.vm.hostname = "katello.#{DOMAIN}"
    katello.vm.network "forwarded_port", guest: 80, host: 9080
    katello.vm.network "forwarded_port", guest: 443, host: 9443
    katello.vm.network "forwarded_port", guest: 5671, host: 5671

    katello.vm.network :private_network, ip: "#{SUBNET}.10"
    katello.vm.provider :virtualbox do |vb|
      vb.gui = false
      vb.customize ["modifyvm", :id, "--memory", 3072]
      vb.customize ["modifyvm", :id, "--ioapic", "on", "--cpus", 2]
    end
    katello.vm.provision "shell",
      inline: "rpm -q wget || yum install -y wget"
    katello.vm.provision "shell",
      inline: "yum upgrade -y"
    katello.vm.provision "shell",
      inline: "rpm -q katello-repos || yum install -y http://fedorapeople.org/groups/katello/releases/yum/1.4/RHEL/6Server/x86_64/katello-repos-1.4.4-1.el6.noarch.rpm"
    katello.vm.provision "shell",
      inline: "rpm -q epel-release-6-8.noarch || yum install -y http://dl.fedoraproject.org/pub/epel/6/x86_64/epel-release-6-8.noarch.rpm"
    katello.vm.provision "shell",
      inline: "wget -O /etc/yum.repos.d/epel-rhsm.repo http://repos.fedorapeople.org/repos/candlepin/subscription-manager/epel-subscription-manager.repo"
    katello.vm.provision "shell",
      inline: "rpm -q katello-foreman-all || yum install -y katello-foreman-all"
    katello.vm.provision "shell",
      inline: "service foreman status | grep -q running || katello-configure --user-pass ChangeM3 -b"
    # katello.vm.provision "shell",
    #  inline: "gem install deep_merge"
    # katello.vm.provision "shell",
    #   inline: "gem install librarian-puppet"
    # katello.vm.provision "shell",
    #   inline: "/usr/bin/yum clean all"
    # katello.vm.provision "shell",
    #   inline: "/usr/bin/yum install -y git"
    # katello.vm.provision "shell",
    #   inline: "cd /vagrant/puppet/ && librarian-puppet install --path=/etc/puppet/modules/  --verbose"
    katello.hostmanager.aliases = %w(puppet puppetmaster)
  end

end
