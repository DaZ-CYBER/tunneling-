
Local Port Forwarding
* Allows use to access remote content that you normally don't have access to
	* Remote Database
	* RDP Server
* Acts almost like a proxy once it goes through Port 22

Remote Port Forwarding
* If you want other people to have access to YOUR local resources
* Basically local port forwarding but flipped

Host 1 (Attacking Machine)
Host 2 (Bridge - Still is not within the firewall)
Host 3 (Victim Machine)

* We CAN'T access Host 3 from Host 1 due to a firewall present that is blocking connections from outside the whitelisted remote machines

* If port 22 is open on Host 2*
* We can bridge a connection from Host 1 to Host 2 through ANY port on Host 1
	* Let's say Port 2001 for example
* We then redirect that traffic through Port 22 on Host 2 towards port 22 on Host 3
	* Note that we WILL NEED TO AUTH AS A USER ON HOST 3 THROUGH SSH*

ssh -g -L2001:Host3:22 user@Host3
ssh -R2001:localhost:22 user@Host3