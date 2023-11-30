 `Get-PnpDevice -PresentOnly | Where-Object { $_.InstanceId -match '^USB' }`

- Was there a USB device connected to the system? (Registry) 
    ```
    SYSTEM\CurrentControlSet\Enum\USB
    Content of Registry key - SYSTEM\CurrentControlSet\Enum\USB
    Content of Registry key - SYSTEM\MountedDevices
As you can see, the GUIDs in the registry entry, which shows when the devices were last mounted. So we can now say that the user used a specific USB device on the system while the student account was logged in.  The correct GUID is 6d781ed1-f08d-4113-82bb-9d4c3a714575
    ```
- Did the user navigate to the USB device? (Shell Bags) 
    ```
    ```
- Was a file moved to the USB device? (Time Stamps with .lnk files) 
    ```
    ```

- Was the file opened on the USB device? (File Opening/Registry)
```
```