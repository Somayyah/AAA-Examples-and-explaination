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

**aaa authorization types**

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
