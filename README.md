# bird-rpki-config
I pretty much followed this guide: https://brooks.sh/2019/11/11/validating-bgp-routes-with-rpki-in-bird/
Install bird:
sudo yum install -y bird2
Edit the bird.conf file to your settings

Add in the bird.conf config to /etc/bird.conf 

Start bird and configure it to start at boot
sudo systemctl start bird.service
sudo systemctl enable bird.service

Command to show all invalids for IPv4 and IPv6:
show route primary table master4 where roa_check(roa_v4, net, bgp_path.last_nonaggregated) = ROA_INVALID

show route primary table master6 where roa_check(roa_v6, net, bgp_path.last_nonaggregated) = ROA_INVALID

Command to reload config after changing bird.conf:
configure soft

Command to show connection to connection to RPKI validator:
show protocols rpki1

You can execute commands like this in the shell to get the list of invalids:
/usr/sbin/birdc 'show route primary table master4 where roa_check(roa_v4, net, bgp_path.last_nonaggregated) = ROA_INVALID' | sed -e '1,2d' | awk {'print $NF,$1'}

