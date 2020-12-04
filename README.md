# AAA-Examples-and-explaination

### Basic aaa authentication command
```#aaa authentication ppp default group tacacs group radius local```<br>
Connection type: PPP<br>
default: applied to all interfaces unless a specific method is specefied to said interface<br>
First authentication method: group tacacs which can contain multiple TACACS Servers<br>
Second authentication method: group radius which can contain multiple RADIUS Servers<br>
Third authentication method: local, user name and password<br><br>
```#aaa authentication ppp method1 group tacacs group radius local```<br>
Applicable only when specefied on an interface<br><br>

**Example From AAA guide by Cisco**<br>
The system administrator uses server groups to specify that only R2 (172.16.2.7) and T2 (172.16.2.77) are valid servers for PPP authentication. To do this, the administrator must define specific server groups whose members are , respectively. In this example, the RADIUS server group “rad2only” is defined as follows using the aaa group server command:<br>
```aaa group server radius rad2only```<br>
```server 172.16.2.7```<br>
The TACACS+ server group “tac2only” is defined as follows using the aaa group server command:<br>
```aaa group server tacacs+ tac2only```<br>
```server 172.16.2.77```<br>
The administrator then applies PPP authentication using the server groups. In this example, the default methods list for PPP authentication follows this order: group rad2only, group tac2only, and local:<br>
```aaa authentication ppp default group rad2only group tac2only local```<br><br>
**aaa login authentication steps**<br>
   1. Write the login authentication rule with the following command:<br>
```aaa authentication login [ default | list-name ] auth_method_1 auth_method_2 ... auth_method_N```<br>
   2. Apply the rule to a specific interface if login rule isn't named default with the following commands:<br>
```line [aux | console | tty | vty] start_number end_number```<br>
```login authentication [list-name]```<br>

**IMPORTANT**<br>
The additional methods of authentication are used only if the previous method returns an error, not if it fails. To specify that the authentication should succeed even if all methods return an error, specify none as the final method in the command line.<br><br>

**Login Authentication Using Enable Password**<br>
To enable password as the login authentication method enter the following command:<br>
```aaa authentication login default enable```<br><br>

**Create special authentication groups**<br>
```aaa group server [method_name] [group_name]```<br>
```server X```<br>
```server Y```<br>
```server Z```<br><br>
**Enabling Password Protection at the Privileged Level**<br>
```aaa authentication enable default method1 [ method2... ]```<br><br>

### AAA Authorization

**aaa authorization types**<br>

1. commands: For EXEC mode commands. EXEC modes are split into two types, user and privilaged modes. Command authorization attempts authorization for all EXEC mode commands, including global configuration commands, associated with a specific privilege level.
2. EXEC: Applies to the attributes associated with a user.
3. Network: Applies to network connections. This can include a PPP, SLIP, or ARAP connection.
4. IP mobile: Applies to authorization for IP mobile services.
5. Configuration: Applies to downloading configurations from the AAA server.
6. Reverse Access: Applies to reverse Telnet sessions.<br>

To create a method list to enable authorization for **all network-related service requests** (including SLIP, PPP,PPP NCPs, and ARAP), use the network keyword.<br>
To create a method list to enable authorization to determine **if a user is allowed to run an EXEC shell**, use the exec keyword.<br>
To create a method list to enable authorization **for specific, individual EXEC commands associated with a specific privilege level**, use the commands keyword. (This allows you to authorize all commands associated
with a specified command level from 0 to 15.)<br>

**Commands**<br>
```Router(config)# aaa authorization {auth-proxy | network | exec | commands level | reverse-access | configuration | ipmobile} {default | list-name} [method1 [method2...]]```<br>
```Router(config)# line [aux | console | tty | vty] line-number [ending-line-number]```<br>
```interface interface-type interface-number```<br>
```Router(config-line)# authorization{arap | commands level | exec | reverse-access} {default | list-name}```<br>
```Router(config-line)# aaa authorization {default | list-name}```<br>
___Examples___<br>

1. Show how to use a TACACS+ server to authorize the use of network services, including PPP and ARA. If the TACACS+ server is not available or an error occurs during the authorization process, the fallback method (none) is to grant all authorization requests.<br>
```Router(config)# aaa authorization network default group tacacs+ none```<br>
2. Shows how to allow network authorization using TACACS+.<br>
```Router(config)# aaa authorization network default group tacacs+```<br>
3. Shows how to configure the router to authorize using RADIUS.<br>
```Router(config)# aaa new-model```<br>
```Router(config)# aaa authorization exec default group radius if-authenticated```<br>
```Router(config)# aaa authorization network default group radius```<br>
```Router(config)# radius-server host IP```<br>
```Router(config)# radius-server key KEY```<br>

### AAA Accounting

**AAA Accounting Types**<br>
1. Network
2. EXEC
3. Command
4. Connection
5. System
6. Resources<br>
**Accounting Record Types**<br>
1. Stop-only: instructs the specified method (RADIUS or TACACS+) to send a stop record accounting notice at the end of the requested user process.
2. Start-stop: start-stop keyword to send a start accounting notice at the beginning of the requested event and a stop accounting notice at the end of the event.
3. none: To stop all accounting activities.<br>

**Command**<br>
```Router(config)#aaa accounting [ network | exec | system | commands level | resource ] [ default | list_name ] [ start-stop | stop-only | none ] method1 method2 ...```<br>
```Router(config)# line [aux | console | tty | vty] line-number [ending-line-number]```<br>
```Router(config-line)# accounting {arap | commands level | connection | exec} {default | list-name}```<br>
```Router(config)#interface interface-type interface-number```<br>
```Router(config-if)# ppp accounting { default | list-name }```<br>

### EXAMPLES

1. Configure a local account on R1, configure authentication on the console and vty lines using local AAA.<br>

```R1(config)# username Admin1 secret admin1pa55```<br>
```R1(config)# aaa new-model```<br>
```R1(config)# aaa authentication login new_set local```<br>
```R1(config)# line console 0```
```R1(config-line)# login authentication new_set```<br>
```R1(config-line)# end```

2. Verify local AAA authentication from the R1 console and the PC-A client.<br>
,,,,,,,,,,,<br>
3. Configure Local AAA Authentication for vty Lines on R1<br>

```R1(config)# ip domain-name cisco.com```<br>
```R1(config)# crypto key generate rsa```<br>
```R1(config)# aaa authentication login SSH-LOGIN local```<br>
```R1(config)# line vty 0 4```<br>
```R1(config-line)# login authentication SSH-LOGIN```
```R1(config-line)# transport input ssh```

4. Configure Server-Based AAA Authentication Using TACACS+ on R2.<br>
```R2(config)# username Admin2 secret admin2pa55```<br>
```R2(config)# tacacs-server host 192.168.2.2```<br>
```R2(config)# tacacs-server key tacacspa55```<br>
```R2(config)# aaa new-model```<br>
```R2(config)# aaa authentication login console_list group tacacs+ local```<br>
```R2(config)# line console 0```<br>
```R2(config-line)# login authentication console_list```<br>

5. Configure Server-Based AAA Authentication Using RADIUS on R3.<br>

```R3(config)# username Admin3 secret admin3pa55```<br>
```R3(config)# radius-server host 192.168.3.2```<br>
```R3(config)# radius-server key KEY```<br>
```R3(config)# aaa authentication login default group radius local```<br>
```R3(config)# console line 0```<br>
```R3(config-line)# login authentication default```
