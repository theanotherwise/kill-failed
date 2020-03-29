# Safe kill process if stop failed

### When ur service (like java) sometimes stuck or failed when stop..


```bash
#!/bin/bash
SERVICE_NAME="example"

PROC_NAME="java"
KEYWORD="example.properties"

systemctl stop "$SERVICE_NAME" || service "$SERVICE_NAME" stop

# if [[ "$?" != "0" ]] ; then
#   echo "Failed stop service.."
# fi

FOUND_PROC=`ps aux | grep -Ei "$PROC_NAME" | grep -Ei "$KEYWORD"`

PROC_KEYWORD=`echo -e "$FOUND_PROC" | grep -Eio "$KEYWORD" | head -1`
PROC_PID=`echo -e "$FOUND_PROC" | grep -Ei "$KEYWORD" | awk '{print $2}'`

if [ "$PROC_KEYWORD" = "$KEYWORD" ] ; then
  kill -9 "$PROC_PID"
fi
```
