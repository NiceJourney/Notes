### useLazyQuery的使用方法

```js
function useLazyQuery(query, options) {
    const ref = React.useRef()

    const [variables, runQuery] = React.useState(false)

    ref.current = useQuery(query, {
        ...options,
        variables,
        skip: !variables
    })

    const runner = (variables) => {
        runQuery(variables)
    }

    return [runner, ref.current]
}
```



**用法:**

```js
const [runQuery, { loading, data }] = useLazyQuery(QUERY, {
    onCompleted: () => doSomething()
})

// ...

const handleClick = (ev) => {
    const variables = { ... }
    runQuery(variables)
}
```



**解释:**

1. 下面这一行的作用是

```js
const [runQuery, { loading, data }] = useLazyQuery(QUERY, {
    onCompleted: () => doSomething()
})


```



2. 这一行



对应到实际项目中: 

> 当前需求: 用户在输入框中输入字符, 按下回车后根据输入的内容在后端进行查找然后返回查找到的数据