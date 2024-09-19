# s920docs
PAX S920 Docs

# Findings
### Getting into the config menu
Pressing 0 on the device should bring up the config menu.

### Default password
System config default password seems to be 123456

### XCB not working?
Check if the XCB service is set to USB or Network in System Config -> XCB Service

### Only Ubuntu 20.04 gcc seems to work, not after
Probably symbol versioning issues. Doing `readelf -V`:
```

Version symbols section '.gnu.version' contains 17 entries:
 Addr: 0x000000000000030e  Offset: 0x0000030e  Link: 3 (.dynsym)
  000:   0 (*local*)       0 (*local*)       2 (GLIBC_2.4)     3 (GLIBC_2.4)  
  004:   3 (GLIBC_2.4)     3 (GLIBC_2.4)     3 (GLIBC_2.4)     3 (GLIBC_2.4)  
  008:   3 (GLIBC_2.4)     3 (GLIBC_2.4)     3 (GLIBC_2.4)     4 (GLIBC_2.34) 
  00c:   3 (GLIBC_2.4)     1 (*global*)      1 (*global*)      1 (*global*)   
  010:   1 (*global*)   

Version needs section '.gnu.version_r' contains 2 entries:
 Addr: 0x0000000000000330  Offset: 0x00000330  Link: 4 (.dynstr)
  000000: Version: 1  File: libc.so.6  Cnt: 2
  0x0010:   Name: GLIBC_2.34  Flags: none  Version: 4
  0x0020:   Name: GLIBC_2.4  Flags: none  Version: 3
  0x0030: Version: 1  File: ld-linux.so.3  Cnt: 1
  0x0040:   Name: GLIBC_2.4  Flags: none  Version: 2
```
The additional GLIBC_2.34 probably causes it to not load. The libc within the device is GLIBC_2.15, so any symbol with version newer than that is not allowed.
