# Use Case #

If you are using Ansible to push configuration templates, there can be a situation where you'd like to push all configuration but like to ignore certain pieces of configuration even though there is a diff. In this example, we are preserving the NTP and username configuration on the device by making the eos_config module skip them. You

# Playbook Execution #

## Config on the switch ##  

```
SWITCH(config)#show run | i username
username random nopassword
username tester nopassword
```


```
SWITCH(config)#show run | i ntp
ntp server 192.168.57.3 source Loopback0
SWITCH(config)#
```

## Snippet from configuration template ##

cat templates/config_template.config

```
ntp server 192.168.100.3 source Loopback0
<snip>
username tester nopassword
```

## Running the playbook ##

```
suresh# ansible-playbook -i hosts git_config_mgmt.yml

TASK [push_diff : Run a diff] *************************************************************************************************************************************
--- before
+++ after
@@ -28,6 +28,9 @@
    150 permit host 192.168.18.1
 ip routing
 ipv6 unicast-routing
+router bgp 65000
+   neighbor 192.168.56.100 remote-as 6500
+   neighbor 192.168.56.100 maximum-routes 12000
 router ospf 1
    max-lsa 12000
 management api http-commands

changed: [192.168.56.12]

<SNIP>
```

As you can see only the BGP configs are identified as different and pushed to the device. The playbook ignore any ntp or username configurations.
