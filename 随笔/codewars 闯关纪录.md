# *2019/6/24*
- # Find The Parity Outlier
> [2, 4, 0, 100, 4, 11, 2602, 36]  
> Should return: 11 (the only odd number)  
>   
> [160, 3, 1719, 19, 11, 13, -21]  
> Should return: 160 (the only even number)  
```
// mine
function findOutlier(int){
  const arr = int.map(v => v%2===0)
  let index = 0
  arr.map(v => {
    if(arr.indexOf(v) === arr.lastIndexOf(v)){
      index = arr.indexOf(v)   
    }
  })
  return int[index]
}
```
```
// the best
function findOutlier(int){
  var even = int.filter(a=>a%2==0);
  var odd = int.filter(a=>a%2!==0);
  return even.length==1? even[0] : odd[0];
}
```

- # Format a string of names like 'Bart, Lisa & Maggie'
> list([ {name: 'Bart'}, {name: 'Lisa'}, {name: 'Maggie'} ])  
> // returns 'Bart, Lisa & Maggie'  
> 
> list([ {name: 'Bart'}, {name: 'Lisa'} ])  
> // returns 'Bart & Lisa'  
> 
> list([ {name: 'Bart'} ])  
> // returns 'Bart'  
> 
> list([])  
> // returns ''  

```
// mine
function list(names){
  const arr = names.map(v => v.name)
  let str = arr.join(', ')
  if(arr.length === 0 || arr.length === 1) return str
  let index = str.lastIndexOf(',')
  const newArr = str.split('')
  newArr.splice(index, 1, ' &')
  return newArr.join('')
}
```
```
// the best
function list(names){
  return names.reduce(function(prev, current, index, array){
    if (index === 0){
      return current.name;
    }
    else if (index === array.length - 1){
      return prev + ' & ' + current.name;
    } 
    else {
      return prev + ', ' + current.name;
    }
  }, '');
 }
```

- # Jaden Casing Strings
> Not Jaden-Cased: "How can mirrors be real if our eyes aren't real"  
> Jaden-Cased:     "How Can Mirrors Be Real If Our Eyes Aren't Real"  

```
// mine
String.prototype.toJadenCase = function () {
  return this.replace(/(^|\s)\S/g, v => v.toUpperCase())
};
```
```
// the best
String.prototype.toJadenCase = function () { 
  return this.split(" ").map(function(word){
    return word.charAt(0).toUpperCase() + word.slice(1);
  }).join(" ");
}
```
# *2019/6/25*
- # Sum of the first nth term of Series
> SeriesSum(1) => 1 = "1.00"  
> SeriesSum(2) => 1 + 1/4 = "1.25"  
> SeriesSum(5) => 1 + 1/4 + 1/7 + 1/10 + 1/13 = "1.57"  

```
// mine
function SeriesSum(n) {
  return [...Array(n).keys()].reduce((acc, cur) => acc + 1 / (1 + cur * 3), 0).toFixed(2)
}
```
```
// the best
function SeriesSum(n, s = 0) {
    return n ? SeriesSum(n - 1, s + 1 / (3 * n - 2)) : s.toFixed(2)
}
```
- # Exes and Ohs(统计相同数量的o和x)
```
// mine
function XO(str) {
    const newStr = str.toLowerCase();
    const obj = {};
    for(let i=0;i<newStr.length;i++){
      obj[newStr[i]] ? obj[newStr[i]]+=1 : obj[newStr[i]]=0
    } 
    return (obj.x===obj.o)
}
```
```
// the best
function XO(str) {
  let x = str.match(/x/gi);
  let o = str.match(/o/gi);
  return (x && x.length) === (o && o.length);
}
```
- # Equal Sides Of An Array
> 找到一个索引N，其中N左边的整数和等于右边的整数和，如果没有请返回 `-1`  
> ```
> Test.assertEquals(findEvenIndex([1,2,3,4,3,2,1]),3, "The array was: [1,2,3,4,3,2,1] \n");
> Test.assertEquals(findEvenIndex([1,100,50,-51,1,1]),1, "The array was: [1,100,50,-51,1,1] \n");
> Test.assertEquals(findEvenIndex([1,2,3,4,5,6]),-1, "The array was: [1,2,3,4,5,6] \n");
> Test.assertEquals(findEvenIndex([20,10,30,10,10,15,35]),3, "The array was: [20,10,30,10,10,15,35] \n");
> ```
```
// mine
function findEvenIndex(arr)
{
  const bool = arr.map((v,i) => {
    const left = arr.slice(0,i)
    const right = arr.slice(i+1,arr.length)
    return (left.length === 0 ? 0 : left.reduce((a,b)=>a+b)) === (right.length === 0 ? 0 : right.reduce((a,b)=>a+b))
  })
  return bool.indexOf(true)
}
```
```
// the best
function findEvenIndex(arr)
{
  for(var i=1; i<arr.length-1; i++) {
    if(arr.slice(0, i).reduce((a, b) =>  a+b) === arr.slice(i+1).reduce((a, b) =>  a+b)) {
      return i;
    }
  }
  return -1;
}
```
- # Does my number look big in this?
> 自恋数是一个数字，它是自己数字的总和，每个数字都增加到给定基数中的数字位数。  
> 1^3 + 5^3 + 3^3 = 1 + 125 + 27 = 153  
> 1^4 + 6^4 + 3^4 + 4^4 = 1 + 1296 + 81 + 256 = 1634  
```
// mine
function narcissistic(value) {
  return value.toString().split('').reduce((acc, cur, index, arr) => acc + Math.pow(cur, arr.length), 0) === value
}
```
```
// the best
function narcissistic( value ) {
  return ('' + value).split('').reduce(function(p, c){
    return p + Math.pow(c, ('' + value).length)
    }, 0) == value;
}
```
# *2019/6/26*
- # IQ Test
>iqTest("2 4 7 8 10") => 3 // Third number is odd, while the rest of the numbers are even  
>iqTest("1 2 1 1") => 2 // Second number is even, while the rest of the numbers are odd  
```
// mine
function iqTest(numbers){
  const arr = numbers.split(' ').map(v => v%2 === 0)
  for(let i=0;i<arr.length;i++){
    if(arr.indexOf(arr[i]) === arr.lastIndexOf(arr[i])){
      return i+1
    }  
  }
}
```
```
// the best
function iqTest(numbers){
  numbers = numbers.split(" ").map(function(el){return parseInt(el)});
  
  var odd = numbers.filter(function(el){ return el % 2 === 1});
  var even = numbers.filter(function(el){ return el % 2 === 0});
  
  return odd.length < even.length ? (numbers.indexOf(odd[0]) + 1) : (numbers.indexOf(even[0]) + 1);
}
```
# *2019/6/27*
- # Find the missing letter
> ['a','b','c','d','f'] -> 'e'  
> ['O','Q','R','S'] -> 'P'  
> (Use the English alphabet with 26 letters!)  
```
// mine
function findMissingLetter(array)
{ 
  const arr = 'abcdefghijklmnopqrstuvwxyz'
  const ARR = 'ABCDEFGHIJKLMNOPQRSTUVWXYZ'
  for (let i=0;i<array.length;i++){
    if(arr.indexOf(array[i]) >= 0) {
      if(arr.indexOf(array[i]) !== arr.indexOf(array[i+1])-1){
        return arr[arr.indexOf(array[i])+1]
      }
    } else {
      if(ARR.indexOf(array[i]) !== ARR.indexOf(array[i+1])-1){
        return ARR[ARR.indexOf(array[i])+1]
      }
    }
    
  }
}
```
```
// the best
function findMissingLetter(array)
{
   var i=array[0].charCodeAt();
   array.map(x=>  x.charCodeAt()==i?i++:i);
   return String.fromCharCode(i);
}
```
- # Roman Numerals Encoder
> solution(1000); // should return 'M'  
> 把数字转化为罗马数字
> ```
> // help
> Symbol    Value
> I          1
> V          5
> X          10
> L          50
> C          100
> D          500
> M          1,000
> ```   
```
// the best
function solution(number){
  // convert the number to a roman numeral
  var  roman = {M:1000,CM:900, D:500,CD:400,C:100,XC:90,L:50,XL:40,X:10,IX:9,V:5,IV:4,I:1 }
  
  var ans = '';
  while(number>0){
      for(a in roman){ 
          if(roman[a]<=number){ ans += a; number-=roman[a]; break;}
              
      }
  }
  return ans;
}
```
