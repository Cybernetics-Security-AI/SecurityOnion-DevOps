# SecurityOnion-DevOps

The sosensorfix script was made because in my personal use of utilizing Security Onion, I run into a problem where the snort.conf file in the /etc/nsm/"sensor name"/ directory fails to pass the snort -c /etc/nsm/"sensor interface name"/snort.conf -T debugging test. I receive a few errors running this test after a fresh install of security onion. These errors include; can't find directory to store snort logs, and can't intialize daq pfring.

After some troubleshooting of the snort.conf file on the sniffing interface directory, I came up with a solution that works.

The original configuration contains the following inside the snort.conf file
config daq: pfring
config daq_dir: /opt/pfring/lib/daq
config daq_var: clusterid="random number"
config daq_var: clustermode=4

I have found that if you change this section to the following it solves the daq pfring intialization error
config daq: pcap
config daq_dir: /usr/include
config daq_var: clusterid="random number"
config daq_mode: passive

Notice you don't have to change the config daq_var: clusterid="random number", the default configuration is fine.

After this change is made, it still can't find snort log directory, so I simply manually added a snort directory in /var/log/
mkdir snort

With these changes your Security Onion Sensor should be working great and pass the pre checks
snort -c /etc/nsm/"sensor interface name"/snort.conf -T

I have made a bash script to automate this process
Note: It will only work after a fresh install
file name is "sosensorfix"
