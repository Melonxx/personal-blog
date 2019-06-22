## 父传子
##### 通过 `props` 传递

```
function Son(props) {
  return (
    <div>
      {props.name} // 这里就是儿子接受到父亲传递的数据
    </div>
  )
}

function Father() {
  return (
    <div>
      <Son name="我是父亲：这是传递的数据" />
    </div>
  )
}

ReactDOM.render(<Father />, document.querySelector('#app'))
```

## 子传父
##### 通过调用 `props` 上的回调函数
```
function Son(props) {
  return (
    <div>
      {props.name('我是儿子：这是传递的数据')}
    </div>
  )
}

function Father() {
  function fn (data) {
    console.log(data) // 这是父亲接收到儿子传递数据的地方
  }
  return (
    <div>
      <Son name={fn} />
    </div>
  )
}

ReactDOM.render(<Father />, document.querySelector('#app'))
```

