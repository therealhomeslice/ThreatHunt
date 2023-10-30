**set location variable**

	use pwd
	export VOLATILITY_LOCATION=file:///path/to/mem.img

**check OS version**
	vol.py -f (image).img imageinfo
	***look at guessed profiles from imageinfo and look at image type (service pack) to “ensure” service pack version***

**set profile variable**

	export VOLATILITY_PROFILE=winXXXXXXXX

find running processes
	vol.py pslist 
	vol.py pstree 
	vol.py psscan

**compare processes linked list for unlinked processes (look at first two columns for a fals)**

	vol.py psxview

look for connections from pids in ps tools and grep for PID you care about 

	vol.py netscan | grep PID 

look for exe running location and command line arguments / pid 

	vol.py cmdline | grep PID 

if you want to see command history buffer or console information output 

	vol.py cmdscan 
	vol.py consoles


list dlls loaded by a process 

	vol.py dlllist -p PID 

count number of dlls loaded by process 

	vol.py dlllist -p pid | grep -i "dll" | wc -l 

find memory pages with excess permissions, not worth doing on Win 10. Look for PAGE_EXECUE_READWRITE and an mz header 

	vol.py malfind > malfind.txt 

create a “exe” from the process will create .exe with name executable.PID.exe 

	vol.py procdump -p PID -D . 

create a mem dump of process 

	vol.py memdump -p PID -D .vol 

if you want to scan with an av (Clamscan) prolly wont be great 

	clamscan -v --scan-pe name.exe 

look for known mutexes of a PID (you must know what your searching for) 

	vol.py handles -t Mutant -p pid -s 

strings search for things in the mem dump 

	strings -af 1234.dmp | grep " " -C 5 -i if you want no case search ip addresses .com @domains 

	vol.py yarascan -Y "Http" the whole mem image 

find reg key for persistence 

	vol.py printkey -K "Microsoft\Windows\CurrentVersion\Run" or RunOnce 

if something odd strings for that file in bad process/mem 

dump hashes to see what may have been stolen 

	vol.py hashdump 

look for file objects in memory recommend piping to grep for a direct search 

	vol.py filescan 
	q 


Extra:
	vol.py -f “/path/to/file” windows.info
	vol.py -f “/path/to/file” windows.pstree
	vol.py -f “/path/to/file” -o “/path/to/dir” windows.dumpfiles ‑‑pid <PID>
	vol.py -f “/path/to/file” -o “/path/to/dir” windows.memmap ‑‑dump ‑‑pid <PID>
	vol.py -f “/path/to/file” windows.handles ‑‑pid <PID>
	vol.py -f “/path/to/file” windows.dlllist ‑‑pid <PID>
	vol.py -f “/path/to/file” windows.cmdline
	vol.py -f “/path/to/file” windows.netstat
	vol.py -f “/path/to/file” windows.malfind
	vol.py -f “/path/to/file” yarascan -y “/path/to/file.yar”
