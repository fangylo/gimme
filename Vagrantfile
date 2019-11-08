require 'vagrant-aws'
require 'vagrant-env'

Vagrant.configure('2') do |config|
  config.env.enable

  config.vm.synced_folder ".", "/vagrant", type: "rsync",
    rsync__exclude: ".git/"

  config.vm.box = 'aws-dummy'
  config.ssh.forward_agent = true
  config.ssh.forward_x11 = true

  config.vm.provider 'aws' do |aws, override|
    aws.ami           = ENV['AWS_AMI']
    aws.instance_type = ENV['AWS_INSTANCE_TYPE']
    aws.keypair_name  = ENV['AWS_KEYPAIR_NAME']
    aws.region        = ENV['AWS_REGION']

    aws.access_key_id     = ENV['AWS_ACCESS_KEY_ID']
    aws.secret_access_key = ENV['AWS_SECRET_ACCESS_KEY']

    aws.security_groups = [
      'SSH From Anywhere'
    ]

    aws.tags = {
        'Name'  => ENV['AWS_INSTANCE_TAG_NAME'],
        'Owner' => ENV['AWS_INSTANCE_TAG_OWNER']
    }
    aws.instance_ready_timeout = 1000

    override.ssh.username         = 'ec2-user'
    override.ssh.private_key_path = ENV['PRIVATE_KEY_PATH']
  end

  config.vm.provision 'ansible' do |ansible|
    ansible.playbook = 'playbook.yml'
    ansible.compatibility_mode = '2.0'
  end

  config.vm.provider "aws" do |aws|
  #   aws.block_device_mapping = [{ 'DeviceName' => '/dev/sda1', 'Ebs.VolumeSize' => 1000 }]
    aws.block_device_mapping = [{ 'DeviceName' => '/dev/xvda', 'Ebs.VolumeSize' => 3000, 'Ebs.VolumeType' => 'gp2' }]
  end

end
