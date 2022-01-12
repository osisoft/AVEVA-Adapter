```bash
#!/bin/sh
if [ -z $portnum ] ; then
    exec /<installer-name>_<symantec-version>-x64_/OSIsoft.Data.System.Host
else
    exec /<installer-name>_<symantec-version>-x64_/OSIsoft.Data.System.Host --port:$portnum
fi
```