学习笔记

我们在数据库操作的时候，比如 dao 层中当遇到一个 sql.ErrNoRows 的时候，是否应该 Wrap 这个 error，抛给上层。为什么，应该怎么做请写出代码？

```go
package main

import (
    "database/sql"
    "fmt"
    "github.com/pkg/errors"
)

func main() {
    err := HaHaService()

    if errors.Cause(err) == sql.ErrNoRows{
        fmt.Printf("%+v\n", err) // 可以打印完整调用栈信息
        return
    } else if err != nil {
        fmt.Printf(err) // 一般情况只普通打印
    }
    
    fmt.Printf("everything is ok!")
}

func HaHaDao() error {
    return errors.Warpf(sql.ErrNoRows, "HaHaDao failed!") // 底层错误包装,增加上下文文本信息&调用栈
}

func HaHaService error {
    return errors.WithMessage(HaHaDao(), "HaHaService failed") // 仅增加上下文信息
}

```

解释:

Dao作为业务最底层应当记录调用栈,向上逐级增加信息

可是说的判断错误 if的判断没用上...学艺不精

