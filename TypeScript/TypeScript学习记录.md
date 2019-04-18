## 接口-interface
```
interface Person {
  firstName: string;
  lastNmae: string;
}

function greeter(person: Person) {
  console.log('Hello'+person.firstName+person.lastName)
}

let user = {firstName: 'zhang', lastName: 'super'};
greeter(user);
```
`person: Person`参数必须符合interface的定义才会被允许。
## Public
为了简化重复申明赋值的属性
```
class Student {
  fullName: string;
  firstName: string;
  lastName: string;
  constructor(firstName, lastName) {
    this.firstName = firstName;
    this.lastName = lastName;
  } 
}
// 因为使用public以上可以等价于以下
class Student {
  fullName: string;
  constructor(pub firstName, public  lastName) {
    this.fullName: firstName + lastName
  } 
}
```
## 类-class
```
class Student {
  fullName: string;
  constructor(public firstName, public  lastName) {
    this.fullName: firstName + lastName
  } 
}
interface Person {
  firstName: string;
  lastName: string;
}
function greeter(person: Person) {
  console.log('Hello'+person.firstName+person.lastName);
}
let user = new Student('zhang', 'super');
greeter(user);
```
## 重载
实现参数多个类型和函数返回值
```
function add(a: string, b: string): string;
function add(a: number, b: number): number;
function add(a: any, b: any): any{
  return a + b
}
```
## 数组
规定了a为number组成的数组
```
function select(a: number[]): number{ // 或者function select(a: Array<string>): number{
  return a[0]
}
```
## 元祖
表示已知一个元素的数量和类型的`数组`
```
let x:[string, number];
x = ['我是string', 1]; // ok
x = [1, '我是string']; // error
x = [1, '我是string', '我还是一个string']; // error
```
## 枚举
定义一些带名字的常量
```
enum Gender{
  male,
  female
}
interface Person {
  sex: Genter;
}
function show( a: Person){
  console.log('我是：'+a.sex);
}
show({sex: Gender.male}) // ok
show({sex: Gender.female}) // ok
show({sex: Gender.hahamale}) // error
show({sex: Gender.11122}) // error
```

持续更新ing