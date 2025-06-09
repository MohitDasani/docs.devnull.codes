## Finding Wi-Fi SSID and Passwords on windows

### Find Available Wi-Fi Profiles (SSIDs)

```cmd
netsh wlan show profile
```

### Retrieve Cleartext Password of a Wi-Fi Profile

```cmd
netsh wlan show profile <SSID> key=clear
```


