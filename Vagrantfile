# -*- mode: ruby -*-
# vi: set ft=ruby nowrap :

def VMConf(config, hostname: "default", box: "generic/ubuntu1804", mounts: nil, ip: nil, cpu: 1, memory: 1024, provisions: nil, customCode: nil, **var)
  config.vm.define hostname do |vh|
    puts("config #{hostname}")
    #==========MACHINE SETTINGS==========
    vh.vm.box = box
    vh.vm.hostname = hostname
    vh.vm.provider :libvirt do |libvirt|
      libvirt.cpus = cpu.to_s
      libvirt.memory = memory.to_s
      libvirt.nested = true
      libvirt.graphics_type = "none"
    end

    #==========NETWORK SETTINGS==========
    # #   cf. https://github.com/vagrant-libvirt/vagrant-libvirt#private-network-options
    #     vh.vm.network :private_network, :type => "dhcp",
    #         :libvirt__network_address => "10.20.30.0", :libvirt__domain_name => "test.local"
    #     vh.vm.network :private_network, ip: "192.168.100.3"
    # #   cf. https://github.com/vagrant-libvirt/vagrant-libvirt#public-network-options
    #     vh.vm.network :public_network, :dev => "virbr1", :mode => "bridge", :type => "bridge"
    #     vh.vm.network :forwarded_port, guest: 80, host: 2000, host_ip: "0.0.0.0"
    # #   cf. https://github.com/vagrant-libvirt/vagrant-libvirt#management-network

    #==========VOLUME SETTINGS==========
    # cf. https://github.com/vagrant-libvirt/vagrant-libvirt#synced-folders
    if mounts != nil
      mounts.each do |vol|
        vh.vm.synced_folder vol[:src], vol[:dst], type: vol[:type]
      end
    end

    #==========PROVISION SETTINGS==========
    #    config.vm.provision :shell, :path => "./common_setup.sh",:privileged => true
    #     vh.vm.provision "shell", inline: script
    if provisions != nil
      provisions.each do |p|
        case p[:type]
        when /shell/
          if p["script"] != nil
            vh.vm.provision :shell, inline: p["script"] #, :privileged => p["privileged"] or false
          end
          if p["file"] != nil
            vh.vm.provision :shell, path: p["file"]
          end
        when /ansible/
          vh.vm.provision "ansible" do |ansible|
            #             ansible.ask_sudo_pass = true
            ansible.playbook = p[:playbook]
            #             ansible.groups = p[:groups] or {}
          end
        else
        end
      end
    end

    #===========CUSTOM CODE==========
    if customCode != nil
      customCode.call(config, vh)
    end
  end
end

Vagrant.configure("2") do |config|
  config.ssh.compression = false
  config.ssh.forward_agent = true
  VMConf(config,
         box: "generic/ubuntu1804",
         cpu: 8,
         memory: 8192,
         mounts: [{ type: "nfs", src: "./", dst: "/home/vagrant/nfs" }],
         provisions: [{ type: :ansible, playbook: "./playbook/default.yml" }])
  #   box: "peru/ubuntu-18.04-server-amd64"
  #   box: "generic/ubuntu1604"
  #   box: "generic/ubuntu1804"
  #   box: "generic/ubuntu2004"
  #   box: "generic/debian8"
  #   box: "generic/debian9"
  #   box: "generic/debian10"
  #   box: "generic/centos7"
  #   box: "generic/centos8"
  #   box: "generic/arch"
  #   box: "generic/gentoo"
  #   box: "generic/fedora31"
  #   box: "generic/fedora32"
  #   box: "CumulusCommunity/cumulus-vx"
  #   box: "CumulusCommunity/VX-3.0"
  #   box: "centos/7"
  #   box: "centos/8"
end
