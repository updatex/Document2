# My Thinking on desin mode

## process-orignted

#### Example 1

*start from break point*
```
BREAK_POIT=$DW_TMP/dw_clsfd/test.break.dat

BREAK_POIT=1
if [ -e $BREAK_FILE ]; then
    BREAK_POIT=$(cat $BREAK_POIT)
else
    echo $BREAK_POIT > $BREAK_POIT
fi

print 'Current Breakpoint is $BREAK_POIT"

if [ $BREAK_POIT -eq 1 ]; then
    commans
    if [ $? -eq 0 ]; then
        echo $(( ++BREAK_POIT )) > BREAK_POIT
        print 'Current Breakpoint is $BREAK_POIT"
    fi
fi

if [ $BREAK_POIT -eq 2 ]; then
    commans
    if [ $? -eq 0 ]; then
        echo $(( ++BREAK_POIT )) > BREAK_POIT
        print "Current Breakpoint is $BREAK_POIT"
    fi
fi

## to be continue ...

```

#### Example 2

*always start from first step*

```
cat commands.dat | while read line
do 
    evale $line
    if [ $? -eq 0 ]; then
        echo "$line failed, please check"
    fi
    print "Step $line succeed"
done

while read line
do 
    eval $line
    if [ $? -ne 0 ]; then
        echo "$line failed, please check"
    fi
    print "Step $line succeed"
done < commands.dat

```

#### other examples

```
1. java local_jar parameters
2. ssh user@host "commands"
3. scp local file user@host:target_path
 or scp user@host:from_path local_path

```

## Bad Points

1. 团队轮子较少，而且不怎么好。可共享的函数，工具，API比较少。大家各做各的。QA的时候，重复测试。
2. 一些任务，必须基于底层构建流程。而不是在某一抽象层做。每次都要看看以前写的东西，粘贴过来。
3. 代码写的太差，不够健壮。效率有时候是个问题。
4. 写代码的思路，总是面向需求，却不考虑可复用，功能点的抽象。




