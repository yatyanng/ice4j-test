The main class is com.example.icetest.PeerA. Please set up NAT environment before running the program. 

# Full Cone NAT 
<pre>
iptables -t nat -A POSTROUTING -o eth1 -j SNAT --to-source "public IP" <br/>
iptables -t nat -A PREROUTING -i eth1 -j DNAT --to-destination "private IP" <br/>
</pre>

# Restricted Cone NAT 
<pre>
iptables -t nat POSTROUTING -o eth1 -p udp -j SNAT --to-source "public IP" <br/>
iptables -t nat PREROUTING -i eth1 -p udp -j DNAT --to-destination "private IP" <br/>
iptables -A INPUT -i eth1 -p udp -m state --state ESTABLISHED,RELATED -j ACCEPT <br/>
iptables -A INPUT -i eth1 -p udp -m state --state NEW -j DROP <br/>
</pre>

# Port Restricted Cone NAT 
<pre>
iptables -t nat -A POSTROUTING -o eth0 -j SNAT --to-source "public IP" <br/>
</pre>

# Symmetric NAT 
<pre>
echo "1" >/proc/sys/net/ipv4/ip_forward <br/>
iptables --flush <br/>
iptables -t nat -A POSTROUTING -o eth1 -j MASQUERADE --random <br/>
iptables -A FORWARD -i eth1 -o eth0 -m state --state RELATED,ESTABLISHED -j ACCEPT <br/>
iptables -A FORWARD -i eth0 -o eth1 -j ACCEPT <br/>
</pre>