
# Iptables
 ### https://www.aparat.com/Salar_Behnia

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
    -F => flash # delete all in chain
    iptables -R [name of chain] [rule number]
    -R => replace 

    -i => inbound interface
    -o => outbound interface
    -j => jump --> -j  [target: ACCEPT | DROP | REJECT | LOG]
    -p => protocol
    --dport=> destination port

    iptables -P INPUT [name of chain] <policy>
    -P => policy (default policy)
    -S => show
    iptables -S # show all tables rule and show default policy
    --------------------------------------------

    [!] -s => source ip --> 192.168.1.1 | 192.168.1.1,192.168.1.5 | 192.168.1.0/24
    -m => match --> -m iprange --src-range 192.168.1.1-192.168.1.200
    ! -s 192.168.1.1 => reverse 
    [!] -d => destination ip
    [!] -p => protocol --> -p tcp | -p tcp,udp,icmp
    [!] -sport => source port --> -sport 7878 | -sport 20:100 | -m multiport -sport 23,21,22
    [!] -dport => destination port

    p tcp --tcp-flags SYN,ACK,FIN SYN -j DROP

    --------------------------------------------
    example:
    iptables -A INPUT -i enp0s3 -p tcp --dport 80 -j DROP
    iptables -R INPUT 2 -i enp0s3 -p tcp --dport 80 -j REJECT
    iptables -R INPUT 2 -i enp0s3 -p tcp --dport 80 -j REJECT --reject-with tcp-reset
    iptables -A INPUT -i enp0s3 -p icmp -j DROP
    iptables -vnL INPUT --line-numbers
    iptables -D INPUT 3
    iptables -F INPUT
    iptables -A OUTPUT -o enp0s3 -p tcp --dport 80 -j LOG --log-prefix "DANGER" 
    iptables -A OUTPUT -o enp0s3 -p tcp --dport 80 -j DROP
    tail -f /var/log/syslog | grep -i DANGER
    --------------------------------------------
    Extension module:
    man iptables-extensions
    iptables -A INPUT -i enp0s3 -p icmp -m comment --comment "admin:hossein" -j DROP
    # limit connection from specified host
    iptables -A INPUT -i enp0s3 -p tcp -m connlimit --connlimit-above 2 -j DROP # more than 2 connections DROP
    iptables -A INPUT -i enp0s3 -p tcp -m connlimit --connlimit-upto 2 -j ACCEPT # less than 2 connections ACCEPT
    iptables -A INPUT -i enp0s3 -p tcp -m mac --mac-source [mac address] -j DROP
    iptables -A INPUT -i enp0s3 -p icmp -m ttl --ttl-eq [eq | gt | lt] 128 -j DROP 
    iptables -A INPUT -i enp0s3 -p icmp -m tcp --mss 1460 -j DROP  # mss : Maximum Segment Size

    # for hardening
    iptables -A OUTPUT -o enp0s3 -p tcp -sport 22 -j ACCEPT
    iptables -A INPUT -i enp0s3 -p tcp -dport 22 -m state --state NEW,ESTABLISHED -j ACCEPT
    # for prevent DDOS
    iptables -A INPUT -i enp0s3 -p tcp -dport 80 -m limit --limit-burst 100 --limit 50/minute -j DROP
    # change default policy
    iptables -P INPUT DROP
    iptables -P OUTPUT DROP
    --------------------------------------------
    Create custom chain:
    iptables [-n | --new] [name of chain like ssh_policy]
    iptables -A ssh_policy -i enp0s3 -p tcp -s 192.168.1.10 --dport 22 -j ACCEPT
    iptables -A ssh_policy -i enp0s3 -p tcp -s 192.168.1.19 --dport 22 -j ACCEPT
    iptables -A ssh_policy -i enp0s3 -p tcp -s 192.168.1.22 --dport 22 -j ACCEPT
    iptables -D ssh_policy 1
    iptables -X ssh_policy # run if don't have any rule in chain
    --------------------------------------------
    Example for server hardening:
    
 
