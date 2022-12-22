# CM - Preliminar Demo

## SDN-1 Project

### Done:

1. OVS Tutorials: Faucet, IPSec, Connection Tracking (some problems, contacted mailing list), OVS Advanced Features.

2. Setup VLANs <br>
    Notation: hij -> host j of VLAN i <br>
    s1-s2: vlans 10, 20, 30 <br>
    s1-s3: vlans 10, 30 <br>
    s3-s2: vlans 20 <br>
    
        h11                 h12 
            \               / 
             \             /  
              \   ____    /
        h21--- s1 ==== s2 ---h22
             / ||     /   \ 
        h24_/  ||    /     \_h32
       (wrong) ||   /
         (IP)  ||  /
               || /
               ||/ 
               s3
              /|\ 
             / | \ 
            /  |  \ 
          h31 h23  h13

    - Use ```pingall``` to test connectivity between hosts. <br>
    - **Note**: <br>
    We saw that the pings between VLANs from h31 to h32 go through s1.

3. GNS3 Setup <br>
    Notation: hij -> host j of VLAN i <br>
    s1-s2: vlans 10, 20 <br>
    
        h11               h12 
           \            /  
            \          /
             s1 ==== s2
            /          \ 
           /            \
        h21              h22
    
4. Setup ACLs <br>
    Notation: hij -> host j of VLAN i <br>
    s1-s2: vlans 10, 20, 30 <br>
    s1-s3: vlans 10, 30 <br>
    s3-s2: vlans 20 <br>
    s2-s3: vlans 10, 20, 30 <br>

        h11     h12  h22  h32   h14  h25  h33
            \       \  |  /        \  |  /
             \       \ | /          \ | /
              \   ____\|/ ___________\|/ 
        h21--- s1 ==== s2 =========== s4 ---- h34 (monitoring)
             / ||     /             (ACLs)
        h24_/  ||    /     
        (wrong)||   /
        (IP)   ||  /
               || /
               ||/ 
               s3
               /|\ 
              / | \ 
             /  |  \ 
           h31 h23  h13
    
    - ACL for host h14 
        - Block IPV4 pings while allowing IPV6 and ARPs.
        - Mirror (needs extra investigation, p. ex. specify a host through its MAC address or IP instead of the switch's port), but redirects traffic blocked to a port.

    - To be tested:
        - ACL for blocking IPs (malicious IPs)
        - ACL for QoS (prioritize IP, specific VLANs, types of packets (p. ex., VoIP vs. ICMP), ...)
        - ACL for blocking P2P File Sharing (for example)

5. We've done something with ONOS!

### Learned

1. Some tutorial feedback (refer to cm.txt with notes)
    Also, waiting for feeback in OVS discussion mailing list

2. Faucet commands to hot-reload .yaml configurations.

3. Debugging with Mininet (xterm to open a console (where we can use TCPdump), wireshark to capture packets, ifconfig, ...).


### To Do

1. GNS3 fix what is done
    Implement a minimal SDN setup in GNS3.<br>
    **Note**: <br> Switches aren't working with the controller, connection to virbr0 (virtual bridge) seems fuzzy
    
2. SDN App (Northbound) <br>
    With the setup of GNS3, use ACLs/Flows to redirect the networks traffic to a specified host that will be monitoring it. <br>
    **Note**: <br>
    No clue what will be done on this app, maybe just a simple Python functionality.

3. Faucet Configurations <br>
    3.1. .yaml ACLs, IPSec and Conntrack - refered above.<br>
    3.2. **Flows** - Explore A LOT more (we've done tutorials but haven't applied this)<br>
    
4. Control and Data network setup <br>
    Create a setup where we have 2 networks, one where there's the normal flow of a SDN network (**DATA**) and the other that has a backup of the controller (**CONTROL**), creating redundancy and failsafe situations. <br>
    We can find this [here](href="https://github.com/mininet/mininet/blob/master/examples/controlnet.py").

5. MiniEdit (mininet UI) / IT2's Hybrid infrasctructure
    Use MiniEdit to show that we can create a hybrid network setup, i.e. using legacy switches and SDN-support switches.

6. ONOS
    Make comparisons of latency, packet drop, queue delay and throughput using two controllers (Faucet and ONOS) with equal network setups.

7. Performance
    Test the perfomance of the network (p. ex. using 100 switches, 100 hosts, which requires recompiling mininet, etc) 
    There's an implementation of this [here](href="https://github.com/mininet/mininet/blob/master/examples/linearbandwidth.py")<br>
    **Note**: <br> Clarify with the profz (is it worth it?)

8. Questions
    Should we implement NAT/PAT in the SDN controller? <br>
    Should we implement a VPN in the SDN controller? <br>
    Should we implement routing (using routers instead of switches) in the SDN controller? <br>
    Why does using UA's network affect the network debugging? <br> 
