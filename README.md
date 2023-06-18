# CVE-2023-29343

This is PoC for arbitrary file write bug in Sysmon version 14.14

![poc](https://github.com/Wh04m1001/CVE-2023-29343/assets/44291883/7793d64b-0382-4e3c-9f20-7adf19cafc9e)




After last patch Sysmon would check if Archive directory exists and if it exists it would check if archive directory is owned by NT AUTHORITY\SYSTEM and access is only granted to NT AUTHORITY\SYSTEM. 
If both conditions are true then Sysmon will write/delete files in that directory.

As its not possible to change ownership of file/directories as a low privilege user I had to find directory that is owned by SYSTEM but gives low privilege user (or any group low privilege user is a member of) full access or at least WRITE_DAC|DELETE|FILE_WRITE_ATTRIBUTES.

I could not find such directory on default installation but was able to create one by abusing Windows service tracing and RasMan service.

This PoC will only work on Sysmon version 14.14 and windows clients before April patch due to changes introduced with patch for CVE-2023-28222 which killed trick i used to create directory that is owned by SYSTEM and grant full access to low privilege user. PoC can be modified to work on clients after April patch if you can abuse other windows services to create directory (or find directories created by third party app's :) )
# References

https://itm4n.github.io/cve-2020-0668-windows-service-tracing-eop/ (@itm4n)
