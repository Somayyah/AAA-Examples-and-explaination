# AAA-Examples-and-explaination

### Basic aaa authentication command
```#aaa authentication ppp default group tacacs group radius local```<br><br>
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
The additional methods of authentication are used only if the previous method returns an error, not if it fails. To specify that the authentication should succeed even if all methods return an error, specify none as the final method in the command line.<br>
