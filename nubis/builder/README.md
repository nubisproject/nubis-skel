
The execute_command for the puppet-masterless provisioners comes from:
https://github.com/mitchellh/packer/blob/master/provisioner/puppet-masterless/provisioner.go#L76 

This was edited to make --modulepath='/etc/nubis-puppet/' for consuming puppet from the base_ami.
