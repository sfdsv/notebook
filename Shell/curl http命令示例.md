```
#!/bin/bash

echo "任务start"

#shellcheck disable=SC1004

result=$(curl -X POST --header 'Content-Type: application/json' --header 'Accept: application/json' -d '{
   "PosName": "PN_SU5",
   "Index": 0
 }' 'http://10.5.10.87:8082/P3/ReplacePCRFromSU5')

echo "任务result:${result}"

if [ "$result" = "{}" ]; then
  echo "success"
  exit 0
else
  echo "failed"
  exit 1
fi
```
