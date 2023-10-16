![image-20231014120453947](D:\htb日记\images\Optinmum\image-20231014120453947.png)

* 10.10.10.8

扫半天啥都没扫到

进去后提示是HFS服务，直接msf启动



![image-20231014213843625](D:\htb日记\images\Optinmum\image-20231014213843625.png)



用户权限



上传winPEAS，看看啥能提权



![image-20231014214137194](D:\htb日记\images\Optinmum\image-20231014214137194.png)

![image-20231014214242171](D:\htb日记\images\Optinmum\image-20231014214242171.png)

```
[+] Files in registry that may contain credentials                                                                                                                                                                                        
   [i] Searching specific files that may contains credentials.                                                                                                                                                                             
   [?] https://book.hacktricks.xyz/windows-hardening/windows-local-privilege-escalation#credentials-inside-files                                                                                                                           
Looking inside HKCU\Software\ORL\WinVNC3\Password                                                                                                                                                                                          
Looking inside HKEY_LOCAL_MACHINE\SOFTWARE\RealVNC\WinVNC4/password                                                                                                                                                                        
Looking inside HKLM\SOFTWARE\Microsoft\Windows NT\Currentversion\WinLogon                                                                                                                                                                  
    LastUsedUsername    REG_SZ    kostas                                                                                                                                                                                                   
    DefaultUsername    REG_SZ    kostas                                                                                                                                                                                                    
    DefaultPassword    REG_SZ    kdeEjDowkS*                                                                                                                                                                                               
Looking inside HKLM\SYSTEM\CurrentControlSet\Services\SNMP                                                                                                                                                                                 
Looking inside HKCU\Software\TightVNC\Server                                                                                                                                                                                               
Looking inside HKCU\Software\SimonTatham\PuTTY\Sessions                                                                                                                                                                                    
Looking inside HKCU\Software\OpenSSH\Agent\Keys                                                                                                                                                                                            
C:\Windows\Panther\setupinfo                                                                                                                                                                                                               
C:\Windows\WinSxS\amd64_ipamprov-dhcp_31bf3856ad364e35_6.3.9600.16384_none_64e8a179c6f2a167\ScheduledTasks.xml                                                                                                                             
C:\Windows\WinSxS\amd64_ipamprov-dns_31bf3856ad364e35_6.3.9600.16384_none_824aabe06aee1705\ScheduledTasks.xml                                                                                                                              
C:\Windows\WinSxS\amd64_microsoft-windows-d..rvices-domain-files_31bf3856ad364e35_6.3.9600.16384_none_8bc96e4517571480\ntds.dit                                                                                                            
C:\Windows\WinSxS\amd64_microsoft-windows-iis-sharedlibraries_31bf3856ad364e35_6.3.9600.16384_none_01a7d2cf88c95dc0\appcmd.exe                                                                                                             
C:\Windows\WinSxS\amd64_microsoft-windows-iis-sharedlibraries_31bf3856ad364e35_6.3.9600.17031_none_01dac51388a3a832\appcmd.exe                                                                                                             
C:\Windows\WinSxS\amd64_microsoft-windows-iis-sharedlibraries_31bf3856ad364e35_6.3.9600.17415_none_01f46dab888fca48\appcmd.exe                                                                                                             
C:\Windows\WinSxS\amd64_microsoft-windows-webenroll.resources_31bf3856ad364e35_6.3.9600.16384_en-us_7427d216367d8d3f\certnew.cer                                                                                                           
C:\Windows\WinSxS\wow64_ipamprov-dhcp_31bf3856ad364e35_6.3.9600.16384_none_6f3d4bcbfb536362\ScheduledTasks.xml                                                                                                                             
C:\Windows\WinSxS\wow64_ipamprov-dns_31bf3856ad364e35_6.3.9600.16384_none_8c9f56329f4ed900\ScheduledTasks.xml                                                                                                                              
C:\Windows\WinSxS\wow64_microsoft-windows-iis-sharedlibraries_31bf3856ad364e35_6.3.9600.16384_none_0bfc7d21bd2a1fbb\appcmd.exe                                                                                                             
C:\Windows\WinSxS\wow64_microsoft-windows-iis-sharedlibraries_31bf3856ad364e35_6.3.9600.17031_none_0c2f6f65bd046a2d\appcmd.exe                                                                                                             
C:\Windows\WinSxS\wow64_microsoft-windows-iis-sharedlibraries_31bf3856ad364e35_6.3.9600.17415_none_0c4917fdbcf08c43\appcmd.exe  
```



豌豆扫到的没有太多想用的东西

然后用msf 的`multi/recon/local_exploit_suggester`试试

https://www.rapid7.com/blog/post/2015/08/11/metasploit-local-exploit-suggester-do-less-get-more/





![image-20231014220125509](D:\htb日记\images\Optinmum\image-20231014220125509.png)

选个直接哒哒哒 



![image-20231014221148189](D:\htb日记\images\Optinmum\image-20231014221148189.png)

成功提权

![image-20231014221231658](D:\htb日记\images\Optinmum\image-20231014221231658.png)