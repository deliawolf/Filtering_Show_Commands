# Filtering_Show_Commands

The show command is one of the primary troubleshooting tools on Cisco IOS networking devices. If not used properly, show commands can provide too much information.

```
SW1# show ip interface brief 
Interface              IP-Address      OK? Method Status             Protocol 
Vlan1                  unassigned      YES NVRAM  up                 up 
Vlan3                  192.168.127.133 YES manual up                 up 
Vlan15                 10.10.15.31     YES NVRAM  up                 up 
Vlan999                192.168.1.31    YES NVRAM  down               down 
FastEthernet0/1        unassigned      YES unset  up                 up 
FastEthernet0/2        unassigned      YES unset  down               down 
FastEthernet0/3        unassigned      YES unset  up                 up 
<... output omitted ...>
```

Common filters that can be used are:

1. exclude
2. include
3. begin
4. section

## Exclude Filter

Use the exclude filter to only display the operational interfaces.
```
SW1# show ip interface brief | exclude down
Interface              IP-Address      OK? Method Status             Protocol 
Vlan1                  unassigned      YES NVRAM  up                 up 
Vlan3                  192.168.127.133 YES manual up                 up 
Vlan15                 10.10.15.31     YES NVRAM  up                 up 
FastEthernet0/1        unassigned      YES unset  up                 up 
<... output omitted ...>
```

In this example, after applying the exclude filter, only the operational interfaces of the previous show ip interface brief command are displayed.

## Include Filter

Use the include filter to show only the lines that include the specified keyword.

Use the include filter to display only the lines that include the specified keyword.
```
R1# show ip route | include 10.10 
O       2.2.2.2 [110/11] via 10.10.20.1, 02:03:25, FastEthernet0/1 
C       10.10.20.0 is directly connected, FastEthernet0/1 
C       10.10.30.0 is directly connected, FastEthernet1/0 
O       10.10.40.0 [110/25] via 172.16.0.2, 02:03:15, Tunnel0 
O       10.10.50.0 [110/35] via 172.16.0.2, 02:03:15, Tunnel0 
O       10.10.60.0 [110/45] via 172.16.0.2, 02:03:15, Tunnel0 
C       10.10.70.0 is directly connected, FastEthernet0/0 
O       10.10.80.0 [110/15] via 172.16.0.2, 02:03:15, Tunnel0
```
In this example, by using the show ip route | include 10.10 command, you show only the lines in the routing table that contain the string “10.10.”

## Begin Filter

The begin filter allows you to skip all output down to the first occurrence of the regular expression pattern.

Use the begin filter to start the listing of output at the first occurrence of a specified keyword.
```
R1# show running-config | begin line vty 
line vty 0
 logging synchronous
 transport input telnet 
line vty 1 4
 access-class ACL_QUIET_ACCESS in
 logging synchronous
 transport input telnet ssh 
line vty 5 10
 access-class ACL_QUIET_ACCESS in
 logging synchronous
 transport input telnet ssh 
! 
end
```
In this example, using the begin filter will start the listing of the show run command output at the first occurrence of the specified keywords, line vty.

## Section Filter

The section filter for the show run command shows the commands inside a matched configuration section.

Use the section filter to display the commands inside a matched configuration section.
```
R1# show running-config | section router 
router ospf 1
 router-id 3.3.3.3
 log-adjacency-changes
 network 10.10.20.0 0.0.0.255 area 0
 network 10.10.30.0 0.0.0.255 area 0
 network 10.20.10.0 0.0.0.255 area 1
 network 10.20.20.0 0.0.0.255 area 1
 network 10.20.30.0 0.0.0.255 area 1
```

## Regular Expressions in Filters

^ = matches the character at the beginning of the string. Example ^234 matches 2345 but not 1234

| = matches one of the character or character pattern on either side of the pipe. this is similar to the logical OR. Example 10|20|30 matches 10,20,30

$ = matches the character or null string at he end of regular expression. Example 123$ matches 0123 but not 01234

* = matches zero or more sequences of the character preceding the asterik. also act as a wildcard for matching any number of character. 2345* matches 2345, 2345 2345, 2345 2345 2345, etc.

Use regular expressions to narrow down the output in show commands to display only relevant information.
```
R1# show interfaces GigabitEthernet0/0 | include ^Giga|errors|packets 
GigabitEthernet0/0 is up, line protocol is up
   5 minute input rate 3543000 bits/sec, 609 packets/sec 
   5 minute output rate 3538000 bits/sec, 604 packets/sec
      1653723282 packets input, 4049140231 bytes, 0 no buffer
      30896 input errors, 0 CRC, 0 frame, 0 overrun, 30896 ignored
      0 input packets with dribble condition detected
      1617318832 packets output, 387518487 bytes, 0 underruns
      0 output errors, 0 collisions, 2 interface resets
```
This example shows the information for the GigabitEthernet0/0 interface. The include filter using a regular expression displays any line that begins with the string “Giga” or any line that includes the strings “errors” or “packets.” The output includes the information on the number of packets per second in the last 5 minutes, input/output errors, and the number of input and output packets in total.

