# -*- mode: ruby -*-
# vi: set ft=ruby :


# Make sure required plugins are installed
# ex: required_plugins = %w({:name => "vagrant-hosts", :version => ">= 2.8.0"})
required_plugins = [{:name => "vagrant-hosts", :version => ">= 2.8.0"}]

plugins_to_install = required_plugins.select { |plugin| not Vagrant.has_plugin?(plugin[:name], plugin[:version]) }
if not plugins_to_install.empty?
  plugins_to_install.each { |plugin_to_install|
    puts "Installing plugin: #{plugin_to_install[:name]}, version #{plugin_to_install[:version]}"
    if system "vagrant plugin install #{plugin_to_install[:name]} --plugin-version \"#{plugin_to_install[:version]}\""
    else
      abort "Installation of one or more plugins has failed. Aborting."
    end
  }
  exec "vagrant #{ARGV.join(' ')}"
end

FIRST_SETUP = <<SCRIPT
echo "==> Installing development tools"
sudo apt -y update
sudo apt -y install git make zsh
SCRIPT

Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/xenial64"
  config.vm.hostname = "dev-box"
  config.vm.synced_folder ".", "/root/dev"
  config.vm.provision "shell", inline: FIRST_SETUP
  config.vm.provision :hosts, :sync_hosts => true
  config.vm.provision :hosts, :add_localhost_hostnames => false
end
