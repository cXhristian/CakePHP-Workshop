require 'rbconfig'

def get_os
    @os ||= (
        host_os = RbConfig::CONFIG['host_os']
        case host_os
        when /mswin|msys|mingw|cygwin|bccwin|wince|emc/
            :"windows"
        when /darwin|mac os/
            :"macosx"
        when /linux/
            :"linux"
        when /solaris|bsd/
            :"unix"
        else
            raise Error::WebDriverError, "unknown os: #{host_os.inspect}"
        end
    )
end

os = get_os.to_s
  
puts "host platform : #{RUBY_PLATFORM} (operative system : #{os})"

machines = {
    # name     => enabled
    :cakephp=> true
}

Vagrant.configure('2') do |config|

    if machines[:cakephp]
        config.vm.define :cakephp do |cakephp_config|

            cakephp_config.vm.box = 'precise32'
            cakephp_config.vm.box_url = 'http://files.vagrantup.com/precise32.box'
            
            cakephp_config.vm.network :forwarded_port, guest: 80, host: 8080
            cakephp_config.vm.network :forwarded_port, guest: 443, host: 8443

            cakephp_config.ssh.forward_agent = true

            # nfs requires static IP
            #cakephp_config.vm.synced_folder '.', '/vagrant/', :nfs => (os == "linux")
            cakephp_config.vm.synced_folder '.', '/vagrant/'

            cakephp_config.vm.provider :virtualbox do |vbox|
                vbox.gui = false
                #vbox.customize ['modifyvm', :id, '--natdnshostresolver1', 'on']
                #vbox.customize ['modifyvm', :id, '--memory', '256']
            end

            #cakephp_config.vm.provision :shell, :path => 'vagrantbootstrap.sh', :privileged => false
        end
    end
end
