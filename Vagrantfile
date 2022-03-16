Vagrant.configure('2') do |config|
  # configure vagrant-proxyconf plugin
  # If proxy is configured via ENV variables
  # then we configure vagrant to use proxy
  unless ENV['http_proxy'].nil?
    config.proxy.enabled  = true
    # set values from environmental variables
    config.proxy.http     = ENV['spbsrv-proxy1.t-systems.ru:3128']
    config.proxy.https    = ENV['spbsrv-proxy1.t-systems.ru:3128']
    config.proxy.no_proxy = ENV['no_proxy']
  end

  config.vm.box = 'bento/centos-7.8'
  config.vm.synced_folder './', '/vagrant', mount_options: ['dmode=775,fmode=644']
  config.vm.network 'forwarded_port', guest: 80, host: 8080

#  config.vm.customize ["modifyvm", :id, "--natdnsproxy1", "off"]
#  config.vm.customize ["modifyvm", :id, "--natdnshostresolver1", "off"]
#  config.vm.customize ["modifyvm", :id, "--natdnspassdomain1", "off"]

  # customisation for virtualbox
  config.vm.provider 'virtualbox' do |box|
    box.name = "centos"
    box.customize ["modifyvm", :id, "--natdnshostresolver1", "off"]
    box.memory = 768
  end

  # provision VM via bash
  config.vm.provision "ansible_local" do |ansible|
    ansible.playbook = "provisioning/playbook.yml"
    ansible.install_mode = "pip"
  end
end
