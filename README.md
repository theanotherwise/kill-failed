# Safe kill process if stop failed

### When ur service (like java) sometimes stuck or failed when stop..


```bash
#!/bin/bash
SERVICE_NAME="example"

PROC_NAME="java"
KEYWORD="example.properties"

/usr/bin/systemctl stop "$SERVICE_NAME" || /usr/sbin/service "$SERVICE_NAME" stop

FOUND_PROC=`ps aux | grep "$PROC_NAME" | grep "$KEYWORD"`

PROC_KEYWORD=`echo -e "$FOUND_PROC" | grep -o "$KEYWORD" | head -1`
PROC_PID=`echo -e "$FOUND_PROC" | grep "$KEYWORD" | awk '{print $2}'`

if [ "$PROC_KEYWORD" = "$KEYWORD" ] ; then
  kill -9 "$PROC_PID"
fi
```
