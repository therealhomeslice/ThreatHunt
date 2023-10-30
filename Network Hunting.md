Executable 
	
	Find executables (MZ Header): tcp.segment_data contains 4d:5a 
	Find GET requests: frame contains 47:45:54

Port scan: 

	tcp > look for grey with black text: sorce IP to dest IP sending SYN packets to various ports on destination icmp > black banner green text (destination unreachable) 
	
	Statistics > ipv4 statistics > all address > sort by count then disregard DC and port scan is likely the top talker 

Find PSEXEC :
	smb2.tid == 0x00000005 
	smb2.tree contains “IPC$” 
	smb2.tree contains “ADMIN$” 

Find Enter-PSSession 

	http.request.full_uri contains “PSVersion” 
	http.user_agent contains “Microsoft WinRM Client” 

Lateral Movement 

	use a filter on a suspected compromised host (ip.addr == <IP>) then 
	look in conversations between hosts in subnet 
	WMI 
		DCERPC 
	C2 
		Look into I/O graph with a focus on the previously ID'd hosts 
		Filter ip.src == <C. IPs> && !(ip.dst == <subnet>) in I/0 Graph 
