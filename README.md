# Safe kill process if stop failed

### When ur service (like java) sometimes stuck or failed when stop..

```bash
#!/bin/bash

SERVICE_NAME="example"
PROC_NAME="java" # case sensitive
KEYWORD="example.properties" # case sensitive, extended regex

/bin/systemctl stop "$SERVICE_NAME" || /usr/sbin/service "$SERVICE_NAME" stop

RES="$?"

FOUND_PROC=`ps -fC "$PROC_NAME" | grep -E "$KEYWORD"`
# FOUND_PROC=`ps aux | grep -E "$PROC_NAME" | grep -E "$KEYWORD"`

PROC_KEYWORD=`echo -e "$FOUND_PROC" | grep -Eo "$KEYWORD" | head -1`
PROC_PID=`echo -e "$FOUND_PROC" | grep -E "$KEYWORD" | awk '{print $2}'`

if [[ "$PROC_KEYWORD" =~ $KEYWORD ]] ; then
  kill -9 "$PROC_PID"
  
  if [[ "$RES" != "0" ]] ; then
    echo -e "Killed '$PROC_PID'\nParsed\n$FOUND_PROC"
  fi
fi
```
