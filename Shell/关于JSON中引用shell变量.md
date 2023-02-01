```shell
for i in {PN_1406_Er,PN_1406_La,PN_1406_Bm}; do
  for j in {1,4,7,10}; do
    url=$path'/P3/ReplaceTip'

    pos="{\"PosName\":\"$i\",\"Index\":$j}"

    result=$(curl -X POST --header 'Content-Type: application/json' --header 'Accept: application/json' -d "$pos" $url)

    echo "任务result:${result}"

    if [ "$result" = "{}" ]; then
      echo "success"
    else
      echo "failed"
      exit 1
    fi
  done
done
```
