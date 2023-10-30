
SPLUNK Search 

User: admin Password: 1qaz@WSX 

index=* [ inputlookup DNT1_Test |table FileName] | lookup DNT1_Test MD5 as MD5 OUTPUT Owner | table FileName,MD5,SHA1,Owner 

index=* [ inputlookup DNT1_Test | table FileName, MD5] | lookup DNT1_Test FileName as FileName MD5 as MD5 OUTPUT Owner, Legit, “Known Bad” | table FileName,MD5,SHA1,Legit, Owner, “Known Bad” 

index=* [ inputlookup DNT1_Test | table MD5] | lookup DNT1_Test MD5 as MD5 OUTPUT Owner, Legit, “Known Bad” | table FileName,MD5,SHA1,Legit, Owner, “Known Bad” index=* | lookup <fileName>.csv File_Name as FileName OUTPUT Owner, Good, Allowed | where Owner = “System” | table FileName, MD5, SHA1, Owner, Good, Allowed 

Autoruns 

index=* source=autoruns Category!=“Office Addins” Category!=“Internet Explorer” Category!=Codecs Category!=Explorer Category!=“Known DLLs” | table host, Category, “Entry Location”, “Launch String” 

Processes 

	index=* source=processes | table host, Pid, ParentPid, Commandline, FilePath

Accounts 

	index=* source=accounts HomeDirectory!=null | table host, AccountType, UserName, HomeDirectory 

NetStat 

	index=* source=netstat LocalAddress!=“0.0.0.0” LocalAddress!=“::” | table host, ConnectionState,LocalAddress, RemoteAddress, ProcessFilePath, Pid, RemoteServiceName logins 

One option is to do a visualization of time vs. logins by going to visualization and selecting 

ShimCache 

index=* source=shimcache | table host, FileLastWriteTime, FilePath, FileExists MD5, SHA1, SHA256 

index=* source=shimcache FilePath IN (*mcbuilder.exe, *windows*) | table host, FileLastWriteTime, FilePath, FileExists MD5, SHA1, SHA256 
