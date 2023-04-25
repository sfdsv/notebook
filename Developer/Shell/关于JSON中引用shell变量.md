```shell
#!/bin/bash

path='http://10.5.10.87:8082'

function foo() {
  for i in $2; do
    for j in $3; do

      pos="{\"PosName\":\"$i\",\"Index\":$j}"

      result=$(curl -X POST --header 'Content-Type: application/json' --header 'Accept: application/json' -d "$pos" $path'/P3/'"$1")

      echo "任务result:${result}"

      if [ "$result" = "{}" ]; then
        echo "success"
      else
        echo "failed"
        exit 1
      fi
    done
  done
}

foo ReplaceTip "PN_1209_Sp PN_1209_Lp" "$(echo {1..12})"
```
