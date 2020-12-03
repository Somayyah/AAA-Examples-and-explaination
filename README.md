# AAA-Examples-and-explaination

### Basic aaa authentication command
```#aaa authentication ppp default group tacacs group radius local```<br><br>
Connection type: PPP<br>
default: applied to all interfaces unless a specific method is specefied to said interface<br>
First authentication method: group tacacs which can contain multiple TACACS Servers<br>
Second authentication method: group radius which can contain multiple RADIUS Servers<br>
Third authentication method: local, user name and password<br>
```#aaa authentication ppp method1 group tacacs group radius local```<br><br>
Applicable only when specefied on an interface<br>
