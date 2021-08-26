# How to do concurrent jobs in shell

## FIFO 

```


function test {
echo $(date) ':' $1
sleep 2
}


function concurrent1 {

start=$1 && end=$2 && cur_num=$3

mkfifo $$.fifo && exec {ff}<>$$.fifo && rm -f $$.fifo

for ((i=$start;i<$cur_num+$start;i++)); do
echo "$i" >&${ff}
done

for ((i=$start;i<=$end;i++)); do

read -u ${ff} reply
{
echo -e "-- current loop: [cmd id: $i, thread id: $reply]"
######### modify here
test
#########
echo $((i%cur_num)) 1>&${ff}
} &
done
wait
}


function concurrent1 {

start=$1 && end=$2 && cur_num=$3

mkfifo $$.fifo && exec {ff}<>$$.fifo && rm -f $$.fifo

for ((i=$start;i<$cur_num+$start;i++)); do
echo "$i" >&${ff}
done

for ((i=$start;i<=$end;i++)); do

read -u ${ff} reply
{
echo -e "-- current loop: [cmd id: $i, thread id: $reply]"
######### modify here
test $i
#########
echo $((i%cur_num)) 1>&${ff}
} &
done
wait
}



function concurrent2 {

start=$1 && end=$2 && cur_num=$3

mkfifo $$.fifo && exec {ff}<>$$.fifo && rm -f $$.fifo

for ((i=$start;i<$cur_num+$start;i++)); do
echo "$i" >&${ff}
done

#### generate command list, or use one function to replace it
:> command.list
for ((i=$start;i<$end;i++))
do
echo $i >> command.list
done
####

cat command.list |grep -v '^$'|while read line
do
read -u ${ff} reply
{
echo -e "-- current loop: [cmd id: $i, thread id: $reply]"
#### modify here
test $line
####
echo $((i%cur_num)) 1>&${ff}
} &
done
wait
}


concurrent1 0 8 3
concurrent2 0 8 3
```

## xargs

xargs 不能使用本文件的函数，需要放到另外一个shell中，共享变量这一块会比fifo差一点。
当然有点也有。xargs毕竟是一个经过验证的工具，在多发可靠性是有保证的。整个代码看起来也更加
简洁清晰。
```
functin generate_cmd {
:> command.list
for ((i=0;i<10;i++))
do
echo $i >> command.list
done
}

cat generate_cmd |grep -v '^$' | xargs -n1 -d '\n' <your comd>

if [$? -eq 0]
then 
  echo 'Succeed'
else
  echo 'Fail'
fi
```

## reference

[Example from here](https://www.cnblogs.com/qinqiao/p/concurrent_by_shell_mkfifo.html)

