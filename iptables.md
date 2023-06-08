
# Iptables

     iptables-save
     iptables-save > /etc/iptables/rules.d/rules.v4
     apt install iptables-persistent 

     iptables -t [name of table] -L # show list of chain in table
     iptables -L # show default 
     iptables -L [name of chain like INPUT] # show default table(filter) with chain

     iptables -L [name of chain like INPUT] -n # show numeric like destination and source ip

     iptables -L [name of chain like INPUT] -v # show verbose
     iptables -nvL [name of chain like INPUT] --line-numbers # show role number
    iptabels -A [name of chain] < -i | -o > [interface name]
    iptabels -I [name of chain] [rule number] < -i | -o > [interface name]
    -A => append
    -I => insert 
    iptables -D [name of chain] [rule number]
    -D => delete
    -F => flash @ delete all in chain

    -i => inbound interface
    -o => outbound interface
    -j => join
    -p => protocol
    --dport=> destination port
