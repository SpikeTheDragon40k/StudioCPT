# Credential Hunting on Windows

## Key Terms to Search


1. Passwords
1. Passphrases
1. Keys
1. Username
1. User account
1. Creds
1. Users
1. Passkeys
1. Passphrases
1. configuration
1. dbcredential
1. dbpassword
1. pwd	Login
1. Credentials

## Using LaZagne

```powershell
C:\Users\xxx\Desktop> start lazagne.exe all
```

### LaZagne Output
```powershell
|====================================================================|
|                                                                    |
|                        The LaZagne Project                         |
|                                                                    |
|                          ! BANG BANG !                             |
|                                                                    |
|====================================================================|


########## User: xxx ##########

------------------- Winscp passwords -----------------

[+] Password found !!!
URL: 10.129.202.51
Login: admin
Password: SteveisReallyCool123
Port: 22
```

## Using findstr
```powershell
C:\> findstr /SIM /C:"password" *.txt *.ini *.cfg *.config *.xml *.git *.ps1 *.yml
```

# Here are some other places we should keep in mind when credential hunting:

1. Passwords in Group Policy in the SYSVOL share
1. Passwords in scripts in the SYSVOL share
1. Password in scripts on IT shares
1. Passwords in web.config files on dev machines and IT shares unattend.xml
1. Passwords in the AD user or computer description fields
1. KeePass databases --> pull hash, crack and get loads of access.
1. Found on user systems and shares
1. Files such as pass.txt, passwords.docx, passwords.xlsx found on user systems, shares, Sharepoint

