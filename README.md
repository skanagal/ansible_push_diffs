# Use Case #

If you are using Ansible to push configuration templates, there can be a situation where you'd like to push all configuration but like to ignore certain pieces of configuration even though there is a diff. In this example, we are preserving the NTP and username configuration on the device by making the eos_config module skip them. You

# Playbook Execution #

## Config on the switch ##  

`SWITCH(config)#show run | i username
username random nopassword
username tester nopassword
`


`SWITCH(config)#show run | i ntp
ntp server 192.168.57.3 source Loopback0
SWITCH(config)#
``
