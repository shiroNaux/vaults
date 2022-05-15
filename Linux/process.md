# Process management

list port: 


```
  netstat -nap | grep {{port-number}}
```

get process id then 

```
  ls -l /proc/{{process-id}}/exe
```