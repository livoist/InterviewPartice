# JavaScript Interview Partice

### Web Render Step
<p>

**Steps**
1. `HTML` => `DOM Tree`
2. `CSS` ⇒ `CSSOM Tree`
3. `DOM Tree and CSSOM Tree` ⇒ `Render Tree`
4. `Render Tree` ⇒ `Layout`
5. `Paint Browser`

</p>

---

### URL Content
<p>

example: `http://www.example.com:80/path/to/myPage?key=value1&key2=value2#someDocument`

1. `Protocol`: 瀏覽器協議，通常都是HTTP or HTTPS ⇒ http。
2. `DomainName`: 域名，對哪個Web進行請求，也可以是IP Address ⇒ www.exapmle.com。
3. `Port`: 連線埠，就像一間公司下的某個部門(分機)。
4. `Path`: 資源路徑，索取資源的地方。
5. `Parameters`: 額外參數，用這些參數來進行額外操作。
6. `Anchor`: 資源下的某一個書籤，定位資源的內容和方向，例如滾動到該anchor。


**輸入網址到渲染畫面的過程**
1. `瀏覽器透過作業系統去找IP網址`
2. `去DNS(查找伺服器)找相對應的IP Address`
3. `把IP Address丟回Browser`


**傳輸協定**
1. `HTTPS(超文本傳輸安全協定)`: 透過HTTP進行通訊且使用SSL/TLS進行加密，S為security，再加上CA證書認證取得安全性，用非對稱加密進行互相溝通，但還是有可能會有中間人攻擊的風險。
2. `中間人攻擊`: A與B之間會有一個C假裝成對方(A或B)，對其中資訊進行攔截與控制，然而雙方卻都不知情。
3. `非對稱加密`: 發送者A與收發者B都會有一組鑰匙，分別是公鑰與私鑰，用其中一把鑰匙所加密的東西只能用另一把鑰匙去解密，舉例，A用B的公鑰加密只有B的私鑰能解密，反之亦然。
5. `TCP協議`: 彼此都會有接收和發送功能，三次握手確認彼此功能正常。

**三次握手**
1. `你有收到嗎?`
2. `我有收到?你有收到嗎?`
3. `我也有收到`

**TCP/IP四層模型(由上到下)**
1. `應用層(Application)`: HTTP、FTP，以上兩個應用層都是使用TCP傳輸模式。
2. `傳輸層(Transport)`: TCP(保證收發正常)、UDP(不保證收發正常、傳輸速度快、效率佳，應用在即時性的訊息)。
3. `網路互連層(Internet)`: IP Address。
4. `網路存取層(Network Access)`: 海底電纜。
</p>

---

### Clourse
```javascript
`Arrow function(Expression)`
const button = (name) => {
  let status = false
  let tpye = name

  return {
    getStatus: () => status,
    getName: () => tpye,
    changeStatus: () => status = !status
  }
}

const n1 = button('Hello')
const n2 = button('World')

n1.changeStatus()
console.log(n1.getStatus())
console.log(n1.getName())

console.log('--------------------------')

console.log(n2.getStatus())
console.log(n2.getName())

`Statement or Declaretion function`
function button(type) {
  let count = 0
  let state = false
  let name = type
  return {
    getCount: () => {
      count++
      console.log('count', count)
    },
    getName: () => console.log('name', name),
    getState: () => console.log('state', state),
    changeState: () => state = !state,
    ob: {
      name: 'hello',
      getThis: function() {
        console.log('name', this)
      }
    }
  }
}

const newBtn1 = button('info1')
newBtn1.getCount()
newBtn1.getName()
newBtn1.changeState()
newBtn1.getState()

console.log('-----------------------')

const newBtn2 = button('info2')
newBtn2.getCount()
newBtn2.getCount()
newBtn2.getName()
newBtn2.changeState()
newBtn2.changeState()
newBtn2.getState()
newBtn2.ob.getThis()
```

---

### this作用域
<p>

誰調用(call function)，this就指向誰(object)，如果沒有就會是global、window、undefiend(全域)
1. `常規函數中有this`
2. `箭頭函數this指向最外層 = 沒有this，或是依所在作用域決定，例如在常規函數中的箭頭函數this就指向常規函數`
3. `setTimeout、setInterval的this則是window`

```javascript
const count1 = {
  count: 10,
  getCount(num) {
    return this.count + num
  },
  getCount2: num => this.count + num
}

const count2 = {
  count: 20,
  getCount(num) {
	return this.count + num
  },
  getCount2: num => this.count + num
}
```

<p>

**call、apply、bind**

1. call、apply、bind都是重新指定this的方式。
2. call是以參數型式傳入、apply則是必須以陣列參數型式傳入，call和apply都是直接執行。
3. bind必須要宣告之後在執行，且參數可在宣告時或是宣告後傳入。
</p>

```javascript
console.log('use call to change count1 this to count2', count1.getCount.call(count2, 50))
console.log('use apply to change count2 this to count1', count2.getCount.apply(count1, [60]))

const newCount = count1.getCount.bind(count2, 30)
console.log('use bind change count1 this to count2', newCount())

const obj = {
  a() { console.log(this) },
  b: () => console.log(this),
  c() {
    console.log(this)

    const c1 = () => console.log(this)
    c1()
  },
  d: () => {
    console.log(this)
    const b1 = () => console.log(this)
  }
}
```

<p>

1. `a`: 陳述式宣告this為所在位置的物件 => obj.a()，this等於obj
完整寫法會是: obj.a.call(obj)

2. `b`: 箭頭函數特性為沒有指向，this為外層作用域 => obj => window
3. `c`: 承a在陳述式中的箭頭函數為外層作用域this => obj.c()的this = obj
4. `d`: 承b在箭頭函式中的箭頭函式一樣沒有指向 => window
</p>


**Class(建構式)**
```javascript
class obj {
  constructor(name) {
	  this.name = name
  }

  a() { console.log(this) }
}

const newOBJ = new obj('Ben')
newOBJ.a()

```
<p>建構式的this會指向自己</p>

**Factory(工廠模式)**
```javascript
function Test() {
  this.name = 'Ben'
}

const n = new Test()
console.log(n.name)
```

<p>工廠模式中的this也是指向自己(常規函數)</p>
</p>

---

### Hosting
<p>如下如果沒有hosting(提升)的話，a就沒有辦法call b function，提升是為了解決函式順序的問題</p>

```javascript
function a() {
	console.log('a')
	b()
}

console.log('call a function', a())

function b() {
	console.log('b')
}
```

---

### 陳述式(Statement or Declaretion): 陳述一段行為不回傳結果
<p>一宣告就會存在記憶體，同時會hosting提升到最上方</p>

```javascript

delFunc()
function delFunc() {}
```

---

### 表達式(Expression): 行為本身會回傳一個結果
<p>

  1. 跟statement差別主要在不會在一宣告就儲存在記憶體裡
  2. 通常是一個計算，會回傳一個值
  3. 暫時性死區: 必須先宣告才能使用 => 沒有hosting
  4. 通常跟元件生命週期一起走
</p>

```javascript
expFunc()
const expFunc = (num) => num * num
```

---

### Front end Q1: 結果為何?
```javascript
class apple {
	static changeColor(newColor) {
		this.newColor = newColor
		return this.newColor
	}

	constructor({ newColor = 'green' } = {}) {
		this.newColor = newColor
	}
}

const newApple = new apple({ newColor: 'red' })
newApple.changeColor('orange')
```
<details><summary><b>A</b></summary>
<p>

`TypeError`

changeColor是一個靜態方法，只能在初始建構式時使用，被能被實例所使用

</p>
</details>

---

### Front end Q2: 結果為何?
```javascript
function woo() {
	console.log('woo woo')
}

woo.people = 'Peter'
```
<details><summary><b>A</b></summary>
<p>正常運作，在JavaScript中除了基本類型之外其他都是對象，函數也只是一個有屬性的對象</p>
</details>

---

### Front end Q3: 結果為何?
```javascript
function Person(FName, LName) {
	this.FName = FName
	this.LName = LName
}

const person = new Person("Lai", "Ben")
person.getFullName = function() {
	return `${this.FName} ${this.LName}`
}

console.log(person.getFullName())
```
<details><summary><b>A</b></summary>
<p>

TypeError，不能替建構函式實例添加任何屬性

hint: 因為並不是每一個子類別都會需要此屬性，所以必須使用Person.prototype.getFullName的方式添加，以免過度浪費空間
</p>
</details>

---

### Front end Q4: 事件的傳播階段?
<details><summary><b>A</b></summary>
<p>Capturing(由上往下) => Target(目標) => Bubbling(由下往上)</p>
</details>

---

### Front end Q5: JavaScript的所有對象都有原型?
<p>JavaScript的所有對象都有原型?</p>
<details><summary><b>A</b></summary>
<p>除了基本類型之外的對象才有原型</p>
</details>

---

### Front end Q6: 類型相加?
```javascript
1. 1 + '2' = '12'
2. '1' + 2 = '12'
3. true + 1 = 2
4. false + 1 = 1
5. true + '1' = 'true1'
```
<details><summary><b>A</b></summary>
<p>'12'、'12'、2、1、'true1'</p>
</details>

---

### Front end Q7: 輸出為何?
```javascript
let number = 0
console.log(number++)
console.log(++number)
console.log(number)
```
<details><summary><b>A</b></summary>
<p>

0、2、2

0 => 後增運算符，下一次才會加上

2 => 前增運算符，當下加上

2 => 最後結果

</p>
</details>

---

### Front end Q8: 輸出為何?
```javascript
function checkAge(data) {
	if (data === { age: 18 }) {
		console.log('A: You are an adult')
	} else if (data === { age: 18 }) {
		console.log('B: You are still an adult')
	} else {
		console.log(`C: Hmm... You don't have an age I guess`)
	}
}

checkAge({ age: 18 })
```
<details><summary><b>A</b></summary>
<p>

在測試相等性時，基本類型通過值(value)進行比較，對象則是用引用(reference)進行比較，
JavaScript會檢查對象的內存位置是否一樣，A、B皆為false，答案為C。

</p>
</details>

---

### Front end Q9: 輸出為何?
```javascript
function getArg(...args) {
	console.log(typeof args)
}
getArg()
```
<details><summary><b>A</b></summary>
<p>object，在JavaScript中所有對象都是物件</p>
</details>

---

### Front end Q10: 輸出為何?
```javascript
function getAge() {
	'use strict'
	age = 21
	console.log(age)
}
getAge()
```
<details><summary><b>A</b></summary>
<p>ReferenceError，未宣告對象</p>
</details>

---

### Front end Q11: cool_secret能訪問多久?
```javascript
sessionStorage.setItem('cool_secret', 123)
```
<details><summary><b>A</b></summary>
<p>當使用者關掉該標籤網頁時，而localStorage則是永久存在，除非去清除他</p>
</details>

---

### Front end Q12: 輸出為何?
```javascript
const obj = { a: 'one', b: 'two', a: 'three' }
console.log(obj)
```
<details><summary><b>A</b></summary>
<p>

{ a: 'three', b: 'two' }

hint: 當物件中出現兩個一樣的key，則會以最後一個key的值為內容，key位置則是不變

</p>
</details>

---

### Front end Q13: 輸出為何?
```javascript
for (let i = 1; i < 5; i++) {
	if (i === 3) continue
	console.log(i)
}
```
<details><summary><b>A</b></summary>
<p>

1、2、4

hint: continue會跳過該次迭代

</p>
</details>

---

### Front end Q14: 輸出為何?
```javascript
const nums = [1, 2, 3]
nums[10] = 10
console.log(nums)
```
<details><summary><b>A</b></summary>
<p>

[1, 2, 3, empty x 7, 10]

hint: 產生7個empty item填補，實際值為undefined，但根據環境也有可能不同

</p>
</details>

---

### Front end Q15: 輸出為何?
```javascript
(() => {
	let x, y
	try {
		throw new Error()
	} catch (x) {
		(x = 1), (y = 2)
		console.log(x)
	}
	console.log(x)
	console.log(y)
})()
```
<details><summary><b>A</b></summary>
<p>

1 undefined 2

catch為區塊作用予以參數x並賦值為1，y則是直接賦值為2
所以在catch之外的x仍然是undefined
</p>
</details>

---

### Front end Q16: JavaScript中一切都是?
<details><summary><b>A</b></summary>
<p>基本類型(boolean、null、undefined、number、string、symbol)與物件</p>
</details>

---

### Front end Q17: 輸出為何?
```javascript
[[0, 1], [2, 3]].reduce((arr, cur) => {
	return arr.concat(cur)
}, [1, 2])
```
<details><summary><b>A</b></summary>
<p>

[1, 2, 0, 1, 2, 3]

hint: 有設定初始值為[1, 2]

</p>
</details>

---

### Front end Q18: !!null、!!''、!!1輸出為何?
<details><summary><b>A</b></summary>
<p>

false false true

null => false => true => false

'' => false => true => false

1 => true => false => true

</p>
</details>

---

### Front end Q19: setInterval返回值是?
<details><summary><b>A</b></summary>
<p>
返回一個ID，可用來clearInterval清除計時器用
</p>
</details>

---

### Front end Q20: 輸出為何?
```javascript
let person = { name: 'Ben' }
const members = [person]
person = null

console.log(members)
```
<details><summary><b>A</b></summary>
<p>

[{ name: 'Peter' }]

引用並不相同，故修改不會造成影響

</p>
</details>

---

### Front end Q21: console.log(3 + 4 + '5')
<details><summary><b>A</b></summary>
<p>
加法運算子從左至右故為3 + 4 = 7，7 + '5'，強制型轉為'75'
</p>
</details>

---

### Front end Q22: 輸出為何?
```javascript
[1, 2, 3].map(num => {
	if (typeof num === 'number') return
	return num * 2
})
```
<details><summary><b>A</b></summary>
<p>

[undefined, undefined, undefined]

在if各自檢查時符合條件，而如果沒有返回任何值，則值默認為undefined

</p>
</details>

---

### Front end Q23: 輸出為何?
```javascript
function getInfo(member, year) {
	member.name = 'Lydia'
	year = '1998'
}

const person = { name: 'Sarah' }
const birthYear = '1997'

getInfo(person, birthYear)

console.log(person, birthYear)
```
<details><summary><b>A</b></summary>
<p>

{ name: 'Lydia' }, '1997'

前者傳參考後者傳值，person的name被修改

</p>
</details>

---

### Front end Q24: 輸出為何?
```javascript
function Car() {
	this.make = 'car1'
	return { make: 'car2' }
}

const myCar = new Car()
console.log(myCar.make)
```
<details><summary><b>A</b></summary>
<p>

'car2'

屬性最後的值會是返回的值

</p>
</details>

---

### Front end Q25: 輸出為何?
```javascript
(() => {
	let x = (y = 10)
})()

console.log(typeof x)
console.log(typeof y)
```
<details><summary><b>A</b></summary>
<p>

undefined number

實際上為

y = 10

let x = y

</p>
</details>

---

### Front end Q26: 輸出為何?
```javascript
class Dog {
	constructor(name) {
		this.name = name
	}
}

Dog.prototype.bark = function() {
	console.log(this.name)
}

const pet = new Dog("Mara")
pet.bark()

delete.Dog.prototype.bark
pet.bark()
```
<details><summary><b>A</b></summary>
<p>

'Mara', TypeError

後者調用一個已經不存在的function，TypeError: pet.bark is not is function

</p>
</details>

---

### Front end Q27: 輸出為何?
```javascript
const arr = [1, 2, 3, 4]
const [a, b, c, d] = arr
console.log('c', c)
```
<details><summary><b>A</b></summary>
<p>
3 => 解構賦值
</p>
</details>

---

### Front end Q28: 輸出為何?
```javascript
const user = { name: 'Ben', age: 32 }
const admin = { admin: true, ...user }

console.log(admin)
```
<details><summary><b>A</b></summary>
<p>

{ admin: true, name: 'Ben', age: 32 }

擴展運算子 => 合併兩個物件

</p>
</details>

---

### Front end Q29: 輸出為何?
```javascript
const settings = {
  username: "lydiahallie",
  level: 19,
  health: 90
};

const data = JSON.stringify(settings, ["level", "health"]);
console.log(data);
```
<details><summary><b>A</b></summary>
<p>

"{"level":19, "health":90}"

JSON.stringify第二個參數為replacer，可以是函數或陣列，用來控制那些值被轉換成string

</p>
</details>

---

### Front end Q30: 輸出為何?
```javascript
let num = 10;

const increaseNumber = () => num++;
const increasePassedNumber = number => number++;

const num1 = increaseNumber();
const num2 = increasePassedNumber(num1);

console.log(num1);
console.log(num2);
```
<details><summary><b>A</b></summary>
<p>

10、10

兩者都是後增運算子

後增運算子(先回傳會累加)

前增運算子(先累加後回傳)

</p>
</details>

---

### Front end Q31: 輸出為何?
```javascript
const value = { number: 10 };

const multiply = (x = { ...value }) => {
  console.log(x.number *= 2);
};

multiply();
multiply();
multiply(value);
multiply(value);
```
<details><summary><b>A</b></summary>
<p>

20、20、20、40

一開始將value的值解構到x當中為默認參數(不一樣的參考)，沒有傳參數時使用

20 => 第一次調用創建新對象(獨立)

20 => 第二次調用一樣創建一個新對象(獨立)

20 => 第三是我們傳入一個object value(會改變傳參考)

40 => 第四次與第三次一樣(在改變一次傳參考)

</p>
</details>

---

### Front end Q32: 輸出為何?
```javascript
[1, 2, 3, 4].reduce((x, y) => console.log(x, y));
```
<details><summary><b>A</b></summary>
<p>

1 2 and undefined 3 and undefined 4

沒有提供initVal則是使用第一個值，第一次為1、2

而每次如果沒有回傳當下值則默認回傳undefined

故接下來為undefined 3 and undefined 4(第一個值為當前累加值)

</p>
</details>

---

### Front end Q33: 正確的構造函數繼承?
```javascript
class Dog {
  constructor(name) {
    this.name = name;
  }
};

class Labrador extends Dog {
  // 1 
  constructor(name, size) {
    this.size = size;
  }
  // 2
  constructor(name, size) {
    super(name);
    this.size = size;
  }
  // 3
  constructor(size) {
    super(name);
    this.size = size;
  }
  // 4 
  constructor(name, size) {
    this.name = name;
    this.size = size;
  }

};
```
<details><summary><b>A</b></summary>
<p>
2為正確答案，必須先以super繼承父類參數，在繼承之前不能使用this，否則將會報錯
</p>
</details>

---

### Front end Q34: 輸出為何?
```javascript
// index.js
console.log('running index.js');
import { sum } from './sum.js';
console.log(sum(1, 2));

// sum.js
console.log('running sum.js');
export const sum = (a, b) => a + b;
```
<details><summary><b>A</b></summary>
<p>

running sum.js, running index.js, 3

import是編譯階段就執行，所以在運行sum function之前就會先運行

</p>
</details>

---

### Front end Q35: 輸出為何?
```javascript
// index.js
console.log('running index.js');
import { sum } from './sum.js';
console.log(sum(1, 2));

// sum.js
console.log('running sum.js');
export const sum = (a, b) => a + b;
```
<details><summary><b>A</b></summary>
<p>

running sum.js, running index.js, 3

import是編譯階段就執行，所以在運行sum function之前就會先運行

</p>
</details>

---

### Front end Q36: 輸出為何?
```javascript
console.log(Number(2) === Number(2))
console.log(Boolean(false) === Boolean(false))
console.log(Symbol('foo') === Symbol('foo'))
```
<details><summary><b>A</b></summary>
<p>

true, true, false

Symbol是唯一的，所以不相等

</p>
</details>

---

### Front end Q37: 輸出為何?
```javascript
const name = "Lydia Hallie"
console.log(name.padStart(13))
console.log(name.padStart(2))
```
<details><summary><b>A</b></summary>
<p>

" Lydia Hallie", "Lydia Hallie"

padStart會在String開頭進行填補(包含填充)

故padStart(13)會在開頭填補一個空格

而如果padStart參數小於長度，則不會進行填補

</p>
</details>

---

### Front end Q38: 輸出為何?
```javascript
console.log("🥑" + "💻");
```
<details><summary><b>A</b></summary>
<p>
"🥑💻" => 字串相加
</p>
</details>

---

### Front end Q39: 輸出為何?
```javascript
async function getData() {
  return await Promise.resolve("I made it!");
}

const data = getData();
console.log(data)
```
<details><summary><b>A</b></summary>
<p>

Promise {<pending>}，異步函式始終返回一個promise，await仍然須等待promise解決，想訪問已解決的值就必須加上.then

data.then(res => console.log(res))

</p>
</details>

---

### Front end Q40: 輸出為何?
```javascript
function addToList(item, list) {
  return list.push(item);
}

const result = addToList("apple", ["banana"]);
console.log(result);
```
<details><summary><b>A</b></summary>
<p>
2，特別注意這邊push是return陣列長度，而不是陣列本身，所以將新對象push進陣列後的長度為2
</p>
</details>

---

### Front end Q41: 輸出為何?
```javascript
const box = { x: 10, y: 20 };

Object.freeze(box);

const shape = box;
shape.x = 100;
console.log(shape)
```
<details><summary><b>A</b></summary>
<p>

TypeError，{ x: 10, y: 20 }

不能對凍結對象進行新增、刪除、修改動作

</p>
</details>

---

### Front end Q42: 輸出為何?
```javascript
const { name: myName } = { name: "Lydia" };

console.log(name);
```
<details><summary><b>A</b></summary>
<p>

ReferenceError，name is not defined

使用解構賦值將右邊的"Lydia"賦值給左邊的myName變數
但並未宣告myName這個變數
</p>
</details>

---

### Front end Q43: 以下是pure function?
```javascript
function sum(a, b) {
  return a + b;
}
```
<details><summary><b>A</b></summary>
<p>
pure function在輸入值時(a、b)，需產生相同的輸出(a、b)，如果有調用其他外部並非由參數傳入的參數就不是
</p>
</details>

---

### Front end Q44: 輸出為何?
```javascript
const add = () => {
  const cache = {};
  return num => {
    if (num in cache) {
      return `From cache! ${cache[num]}`;
    } else {
      const result = num + 10;
      cache[num] = result;
      return `Calculated! ${result}`;
    }
  };
};

const addFunction = add();
console.log(addFunction(10));
console.log(addFunction(10));
console.log(addFunction(5 * 2));
```
<details><summary><b>A</b></summary>
<p>

Calculated! 20 From cache! 20 From cache! 20

在return的function中依賴於外面的cache，所以會被緩存不會被釋放
因此只有在執行第一次的時候是empty object，第二與第三次則都是已緩存在其中
</p>
</details>

---

### Front end Q45: 輸出為何?
```javascript
const myLifeSummedUp = ["☕", "💻", "🍷", "🍫"]

for (let item in myLifeSummedUp) {
  console.log(item)
}

for (let item of myLifeSummedUp) {
  console.log(item)
}
```
<details><summary><b>A</b></summary>
<p>

0 1 2 3 and "☕" "💻" "🍷" "🍫"

for in => 可遍歷一個對象可枚舉的屬性，陣列可枚舉屬性為key故為index: 0 1 2 3

for of => 迭代可迭代對象(Array、Map、Set、String、arguments)，故為: "☕" "💻" "🍷" "🍫"

</p>
</details>

---

### Front end Q46: 輸出為何?
```javascript
const list = [1 + 2, 1 * 2, 1 / 2]
console.log(list)
```
<details><summary><b>A</b></summary>
<p>

3，2，0.5

陣列中可包含任何值(number、string、boolean、object、array、null、undefined)以及任何表達式(日期、函數、計算)

</p>
</details>

---

### Front end Q47: 輸出為何?
```javascript
function sayHi(name) {
  return `Hi there, ${name}`
}

console.log(sayHi())
```
<details><summary><b>A</b></summary>
<p>

Hi there, undefined

在沒有給參數傳值時，預設值都式undefined

</p>
</details>

---

### Front end Q48: 輸出為何?
```javascript
var status = "😎"

setTimeout(() => {
  const status = "😍"

  const data = {
    status: "🥑",
    getStatus() {
      return this.status
    }
  }

  console.log(data.getStatus())
  console.log(data.getStatus.call(this))
}, 0)
```
<details><summary><b>A</b></summary>
<p>

"🥑" and "😎"

data.getStatus() => data調用getStatus，this指向data = 🥑

data.getStatus.call(this) => 使用call重新指定this為全局this = 😎

</p>
</details>

---

### Front end Q49: 輸出為何?
```javascript
const person = {
  name: "Lydia",
  age: 21
}

let city = person.city
city = "Amsterdam"

console.log(person)
```
<details><summary><b>A</b></summary>
<p>

{ name: "Lydia", age: 21 }

這邊特別注意並沒有引用person這個object，只是宣告一個city並給予person上一個不存在的key
所以並不會影響person object本身
</p>
</details>

---

### Front end Q50: 輸出為何?
```javascript
function checkAge(age) {
  if (age < 18) {
    const message = "Sorry, you're too young."
  } else {
    const message = "Yay! You're old enough!"
  }

  return message
}

console.log(checkAge(21))
```
<details><summary><b>A</b></summary>
<p>
ReferenceError，if判斷式之外並沒有message這個變數，變數message只存在區塊作用域中
</p>
</details>

---

### Front end Q51: 輸出為何?
```javascript
function getName(name) {
  const hasName = //
}
```
<details><summary><b>A</b></summary>
<p>
!!name，邏輯非運算符，如果name存在則!name回傳false，而再使用一次運算符確認為真!!name回傳true
</p>
</details>

---

### Front end Q52: 輸出為何?
```javascript
// module.js 
export default () => "Hello world"
export const name = "Lydia"

// index.js 
import * as data from "./module"

console.log(data)
```
<details><summary><b>A</b></summary>
<p>

{ default: function default(), name: "Lydia" }

導入所有關鍵字*，並賦名為data

</p>
</details>

---

### Front end Q53: 輸出為何?
```javascript
class Person {
  constructor(name) {
    this.name = name
  }
}

const member = new Person("John")
console.log(typeof member)
```
<details><summary><b>A</b></summary>
<p>

object

class是構造函數的語法糖，用構造函數寫則是

function Person() {
  this.name = name
}

</p>
</details>

---

### Front end Q54: 輸出為何?
```javascript
let newList = [1, 2, 3].push(4)

console.log(newList.push(5))
```
<details><summary><b>A</b></summary>
<p>
Error，.push(4)回傳的length，故報錯
</p>
</details>

---

### Front end Q55: 輸出為何?
```javascript
function giveLydiaPizza() {
  return "Here is pizza!"
}

const giveLydiaChocolate = () => "Here's chocolate... now go hit the gym already."

console.log(giveLydiaPizza.prototype)
console.log(giveLydiaChocolate.prototype)
```
<details><summary><b>A</b></summary>
<p>

{ constructor: ...} undefined

giveLydiaPizza => 常規函數有prototype屬性，是個有帶constructor的對象

giveLydiaChocolate => 箭頭函數沒有prototype屬性，回傳undefined

</p>
</details>

---

### Front end Q56: 輸出為何?
```javascript
const person = {
  name: "Lydia",
  age: 21
}

for (const [x, y] of Object.entries(person)) {
  console.log(x, y)
}
```
<details><summary><b>A</b></summary>
<p>

["name", "Lydia"] and ["age", 21]

Object.entries方法可枚舉一個對象身上可枚舉的key and value
然後再應用for of loop迭代所有對象
再使用[x, y]方式解構出兩個對象
</p>
</details>

---

### Front end Q57: 輸出為何?
```javascript
function getItems(fruitList, ...args, favoriteFruit) {
  return [...fruitList, ...args, favoriteFruit]
}

getItems(["banana", "apple"], "pear", "orange")
```
<details><summary><b>A</b></summary>
<p>
SyntaxError，剩餘參數包含剩下的所有參數請只能做為最後一個參數傳入，上設置為第二個，故報錯誤
</p>
</details>

---

### Front end Q58: 輸出為何?
```javascript
function getItems(fruitList, ...args, favoriteFruit) {
  return [...fruitList, ...args, favoriteFruit]
}

getItems(["banana", "apple"], "pear", "orange")
```
<details><summary><b>A</b></summary>
<p>

SyntaxError，剩餘參數包含剩下的所有參數請只能做為最後一個參數傳入，上設置為第二個，故報錯誤

如果將傳入參數位置對調則能得到正確結果 => [ 'banana', 'apple', 'orange', 'pear' ]

</p>
</details>

---

### Front end Q59: 輸出為何?
```javascript
function nums(a, b) {
  if
  (a > b)
  console.log('a is bigger')
  else 
  console.log('b is bigger')
  return 
  a + b
}

console.log(nums(4, 2))
console.log(nums(1, 2))
```
<details><summary><b>A</b></summary>
<p>

a is bigger, undefined and b is bigger, undefined

在return後的a + b並不會執行，return沒有預設值，故為undefined

</p>
</details>

---

### Front end Q60: 輸出為何?
```javascript
const info = {
  [Symbol('a')]: 'b'
}

console.log(info)
console.log(Object.keys(info))
```
<details><summary><b>A</b></summary>
<p>

{Symbol('a'): 'b'} and []

Symbol是不可枚舉、不可見的，故Object.keys()會返回一個空陣列，因為沒有可以枚舉的key

如果要訪問Symbol對象的屬性時可使用Object.getOwnPropertySymbols()

</p>
</details>

---

### Front end Q61: 輸出為何?
```javascript
const getList = ([x, ...y]) => [x, y]
const getUser = user => { name: user.name, age: user.age }

const list = [1, 2, 3, 4]
const user = { name: "Lydia", age: 21 }

console.log(getList(list))
console.log(getUser(user))
```
<details><summary><b>A</b></summary>
<p>

[1, [2, 3, 4]] and SyntaxError

getList(list) => [x, ...y] = [1, 2, 3, 4]，但傳入後對y參數來說就會是一個陣列[2, 3, 4]

getUser(user) => 箭頭函數只返回一個值不用想括號，但如果要返回一個對象加上一個圓括號

ex: const getUser = user => ({ name: user.name, age: user.age })

</p>
</details>

---

### Front end Q62: 輸出為何?
```javascript
const name = "Lydia"

console.log(name())
```
<details><summary><b>A</b></summary>
<p>
TypeError，name is not a function
</p>
</details>

---

### Front end Q63: 輸出為何?
```javascript
const output = `${[] && 'Im'}possible!
You should${'' && `n't`} see a therapist after so much JavaScript lol`
```
<details><summary><b>A</b></summary>
<p>

Impossible! You should see a therapist after so much JavaScript lol

邏輯與運算子: 滿足所有條件並回傳最後一個真值

[] && 'Im' => 兩者都為true則回傳後者的值(true && true)

'' && `n't` => 前者值(任一值)為false則不滿足條件，不回傳任何值(false && true or true && false)

</p>
</details>

---

### Front end Q64: 輸出為何?
```javascript
const one = (false || {} || null)
const two = (null || false || "")
const three = ([] || 0 || true)

console.log(one, two, three)
```
<details><summary><b>A</b></summary>
<p>

邏輯或運算子: 滿足其中條件，並回傳第一個真值，若所有條件為偽值則回傳最後一個值

(false || {} || null) => false || true || false = {}

(null || false || "") => false || false || false = ""

([] || 0 || true) => true || false || true = []

ex: 型別補充

undefined => false

null => false

object => true

array => true

'' => false

' ' => true

0 => false

10 => true

</p>
</details>

---

### Front end Q65: 輸出為何?
```javascript
const myPromise = () => Promise.resolve('I have resolved!')

function firstFunction() {
  myPromise().then(res => console.log(res))
  console.log('second')
}

async function secondFunction() {
  console.log(await myPromise())
  console.log('second')
}

firstFunction()
secondFunction()
```
<details><summary><b>A</b></summary>
<p>

second, I have resolved! and I have resolved!, second

兩者運作方式不同

firstFunction => myPromise會被加到任務佇列，因此second會先被印出來結束後才執行任務住列中的myPromise

secondFunction => 透過await關鍵字將會等myPromise reslove才執行second

</p>
</details>

---

### Front end Q66: 輸出為何?
```javascript
const set = new Set()

set.add(1)
set.add("Lydia")
set.add({ name: "Lydia" })

for (let item of set) {
  console.log(item + 2)
}
```
<details><summary><b>A</b></summary>
<p>

3, "Lydia2", "[Object object]2"

+運算符不僅能用來相加數值也能用來連接字串

set.add(1) => 1 + 2 = 3

set.add("Lydia") => "Lydia2" = 強制型轉字串化

set.add({ name: "Lydia" }) => 兩者都不是字串，將兩者都字串化並相加

</p>
</details>

---

### Front end Q67: 輸出為何?
```javascript
Promise.resolve(5)
```
<details><summary><b>A</b></summary>
<p>

Promise {<fulfilled>: 5}

回傳一個已解決的task = 5

</p>
</details>

---

### Front end Q68: 輸出為何?
```javascript
function compareMembers(person1, person2 = person) {
  if (person1 !== person2) {
    console.log("Not the same!")
  } else {
    console.log("They are the same!")
  }
}

const person = { name: "Lydia" }

compareMembers(person)
```
<details><summary><b>A</b></summary>
<p>

They are the same!

引用相同(by reference)

</p>
</details>

---

### Front end Q69: 輸出為何?
```javascript
const colorConfig = {
  red: true,
  blue: false,
  green: true,
  black: true,
  yellow: false,
}

const colors = ["pink", "red", "blue"]

console.log(colorConfig.colors[1])
```
<details><summary><b>A</b></summary>
<p>

TypeError

未定義對象無法訪問

</p>
</details>

---

### Front end Q70: 那些陣列被修改了?
```javascript
const emojis = ['✨', '🥑', '😍']

emojis.map(x => x + '✨')
emojis.filter(x => x !== '🥑')
emojis.find(x => x !== '🥑')
emojis.reduce((acc, cur) => acc + '✨')
emojis.slice(1, 2, '✨') 
emojis.splice(1, 2, '✨')
```
<details><summary><b>A</b></summary>
<p>

splice

map、filter、slice會return一個新陣列，
find return一個屬性，
reduce return一個縮減後的值
</p>
</details>

---

### Front end Q71: 輸出為何?
```javascript
const food = ['🍕', '🍫', '🥑', '🍔']
const info = { favoriteFood: food[0] }

info.favoriteFood = '🍝'

console.log(food)
```
<details><summary><b>A</b></summary>
<p>

['🍕', '🍫', '🥑', '🍔']

將info object的key賦值(by value)，沒有更改到原來的陣列

</p>
</details>

---

### Front end Q72: JSON.stringify and JSON.parse

<details><summary><b>A</b></summary>
<p>
JSON.stringify([1, 2, 3]) => "[1, 2, 3]"
JSON.parse("[1, 2, 3]") => [1, 2, 3]

JSON.stringify({ name: '123' }) => "'{ "name": "123" }'
JSON.parse("'{ "name": "123" }') => { name: '123' }
</p>
</details>

---

### Front end Q73: 輸出為何?
```javascript
let name = 'Lydia'

function getName() {
  console.log(name)
  let name = 'Sarah'
}

getName()
```
<details><summary><b>A</b></summary>
<p>
ReferenceError，暫時性死區、區塊作用域
</p>
</details>

---

### Front end Q74: 輸出為何?
```javascript
console.log(`${(x => x)('I love')} to program`)
```
<details><summary><b>A</b></summary>
<p>

I love to program

IFEE(立即執行函式)

(x => x)('I love') => 將'I love'參數給予(x => x)並執行

</p>
</details>

---

### Front end Q75: 輸出為何?
```javascript
let config = {
  alert: setInterval(() => {
    console.log('Alert!')
  }, 1000)
}

config = null
```
<details><summary><b>A</b></summary>
<p>

setInterval 的回调会被每秒钟调用

箭頭函數會被綁定在context之中也就是config object，所以不會被回收會一直執行

</p>
</details>

---

### Front end Q76: 輸出為何?
```javascript
const person = {
  name: "Lydia",
  age: 21
}

const changeAge = (x = { ...person }) => x.age += 1
const changeAgeAndName = (x = { ...person }) => {
  x.age += 1
  x.name = "Sarah"
}

changeAge(person)
changeAgeAndName()

console.log(person)
```
<details><summary><b>A</b></summary>
<p>

{name: "Lydia", age: 22}

changeAge(person) => 傳入對象並更改(by reference)

changeAgeAndName() => 解構對象，參考值不同不會被更改

</p>
</details>

---

### Front end Q77: 輸出為何?
```javascript
let num = 1;
const list = ["🥳", "🤠", "🥰", "🤪"];

console.log(list[(num += 1)]);
```
<details><summary><b>A</b></summary>
<p>
🥰
陣列裡對象可為計算式
</p>
</details>

---

### Front end Q78: 輸出為何?
```javascript
const groceries = ["banana", "apple", "peanuts"];

if (groceries.indexOf("banana")) {
	console.log("We have to buy bananas!");
} else {
	console.log(`We don't have to buy bananas!`);
}
```
<details><summary><b>A</b></summary>
<p>

We don't have to buy bananas

groceries.indexOf("banana") = 0 = false

</p>
</details>

---

### Front end Q79: 輸出為何?
```javascript
const name = "Lydia Hallie";

console.log(!typeof name === "object");
console.log(!typeof name === "string");
```
<details><summary><b>A</b></summary>
<p>

false false

typeof string = true，!true = false，false === "object" = false

</p>
</details>

---

### Front end Q80: 輸出為何?
```javascript
const add = x => y => z => {
	console.log(x, y, z);
	return x + y + z;
};

add(4)(5)(6);
```
<details><summary><b>A</b></summary>
<p>

4 5 6

返回箭頭函數的箭頭函數，每個都有獨立的作用域

</p>
</details>

---

### Front end Q81: 輸出為何?
```javascript
const myFunc = ({ x, y, z }) => {
	console.log(x, y, z);
};

myFunc(1, 2, 3);
```
<details><summary><b>A</b></summary>
<p>

undefined undefined undefined

期望會收到一個帶有三個屬性的object，但卻沒有收到，故都回傳默認值undefined

</p>
</details>

---

### Front end Q82: 輸出為何?
```javascript
const spookyItems = ["👻", "🎃", "🕸"];
({ item: spookyItems[3] } = { item: "💀" });

console.log(spookyItems);
```
<details><summary><b>A</b></summary>
<p>

["👻", "🎃", "🕸", "💀"]

解構賦值

將右邊的key值賦值給左邊一樣key值的人(spookyItems[3])

</p>
</details>

---

### Front end Q83: 輸出為何?
```javascript
const name = "Lydia Hallie";
const age = 21;

console.log(Number.isNaN(name));
console.log(Number.isNaN(age));

console.log(isNaN(name));
console.log(isNaN(age));
```
<details><summary><b>A</b></summary>
<p>

false false true false

方法Number.isNaN(Not-A-Number)可檢查一個值是否為數字並且等於NaN

Number.isNaN(name) => false

Number.isNaN(age) => age為數字但不等於NaN

isNaN(name) => 他不是一個數字 = true

isNaN(age) => 他是一個數字 = false

</p>
</details>

---

### Front end Q84: 輸出為何?
```javascript
const randomValue = 21;

function getInfo() {
	console.log(typeof randomValue);
	const randomValue = "Lydia Hallie";
}

getInfo();
```
<details><summary><b>A</b></summary>
<p>
ReferenceError，暫時性死區，如果宣告同樣名稱變數在不同作用域時，會以當下作用域為主，故產生TDZ(temp dead zone)
</p>
</details>

---

### Front end Q85: 輸出為何?
```javascript
const myPromise = Promise.resolve("Woah some cool data");

(async () => {
	try {
		console.log(await myPromise);
	} catch {
		throw new Error(`Oops didn't work`);
	} finally {
		console.log("Oh finally!");
	}
})();
```
<details><summary><b>A</b></summary>
<p>

Woah some cool data Oh finally!

首先try區塊會先執行Woah some cool data，而沒有錯誤不會catch，而finally總是執行Oh finally!

</p>
</details>

---

### Front end Q86: 輸出為何?
```javascript
const emojis = ["🥑", ["✨", "✨", ["🍕", "🍕"]]];

console.log(emojis.flat(1));
```
<details><summary><b>A</b></summary>
<p>

['🥑', '✨', '✨', ['🍕', '🍕']]

flat(1)方法攤平一層陣列

</p>
</details>

---

### Front end Q87: 輸出為何?
```javascript
class Counter {
	constructor() {
		this.count = 0;
	}

	increment() {
		this.count++;
	}
}

const counterOne = new Counter();
counterOne.increment();
counterOne.increment();

const counterTwo = counterOne;
counterTwo.increment();

console.log(counterOne.count);
```
<details><summary><b>A</b></summary>
<p>
3

前者創建實例，並且執行兩次increment後count為2，
後者宣告新變數counterTwo且引用counterOne，故會影響最先的引用counterOne(class為物件也是傳參考)
</p>
</details>

---

### Front end Q88: 輸出為何?
```javascript
const myPromise = Promise.resolve(Promise.resolve("C"));

function funcOne() {
	myPromise.then(res => res).then(res => console.log(res));
	setTimeout(() => console.log("AT"), 0);
	console.log("A");
}

async function funcTwo() {
	const res = await myPromise;
	console.log(await res);
	setTimeout(() => console.log("BT"), 0);
	console.log("B");
}

funcOne();
funcTwo();
```
<details><summary><b>A</b></summary>
<p>
Last line! Promise! Promise! Last line! Timeout! Timeout!

1. 首先執行funcOne，myPromise和setTimeout都是非同步會被放到任務佇列，先執行A

2. funcOne任務結束myPromise已經resolve執行C

3. 接著執行funcTwo，await myPromise，執行C

4. 碰到setTimeout放到任務佇列

5. 接著執行B

6. 最後執行兩個setTimeout，先AT再BT(任務佇列順序優先)

result: A C C B AT BT

當任務佇列碰到promise和setTimeout時，promise優先度較高

</p>
</details>

---

### Front end Q89: 輸出為何?
```javascript
const handler = {
	set: () => console.log("Added a new property!"),
	get: () => console.log("Accessed a property!")
};

const person = new Proxy({}, handler);

person.name = "Lydia";
person.name;
```
<details><summary><b>A</b></summary>
<p>

Added a new property! Accessed a property!

person.name = "Lydia"等於會觸發handler的set

person.name等於會觸發handler的get

</p>
</details>

---

### Front end Q90: 以下何者沒有副作用?
```javascript
const person = {
  name: 'Ben',
  address: {
    street: '100 Main St'
  }
}

Object.freeze(person)
```
<details><summary><b>A</b></summary>
<p>
C => 使用Object.freeze對一個物件進行淺凍結，只凍結那個物件本身不包含物件底下的另一個物件，C為真

A: person.name = 'new Name'
B: delete person.address
C: person.address.street = '101 Main St'
D: person.pet = { name: 'mara' }
</p>
</details>

---

### Front end Q91: 輸出為何?
```javascript
const add = x => x + x

function myFunc(num = 2, value = add(num)) {
  console.log(num, value)
}

myFunc()
myFunc(3)
```
<details><summary><b>A</b></summary>
<p>
2、4，3、6

默認參數

第一次myFunc前者參數默認值為2，後者回傳x + x
第二次myFunc(3)改變前者默認值，也一起影響第二個參數回傳的結果

ex: 其餘參數與默認參數，其餘參數必須是最後一個參數，否則會報錯，默認參數沒有限制，但如果沒有傳遞默認就會是undefined
</p>
</details>

---

### Front end Q92: 輸出為何?
```javascript
class Counter {
  #number = 10

  increment() {
    this.#number++
  }

  getNum() {
    return this.#number
  }
}

const counter = new Counter()
counter.increment()

console.log(counter.#number)
```
<details><summary><b>A</b></summary>
<p>
SyntaxError

ES2020語法，添加私有變數(#number)，無法從外部獲取該值
</p>
</details>

---

### Front end Q93: 輸出為何?
```javascript
class Bird {
  constructor() {
    console.log("I'm a bird. 🦢")
  }
}

class Flamingo extends Bird {
  constructor() {
    console.log("I'm pink. 🌸")
    super()
  }
}

const pet = new Flamingo()
```
<details><summary><b>A</b></summary>
<p>
I'm pink. 🌸 I'm a bird. 🦢

創建實體Flamingo，Flamingo的constructor被調用，輸出I'm pink. 🌸，之後再調用super()，super調用父類的建構式，印出I'm a bird. 🦢
</p>
</details>

---

### Front end Q94: 輸出為何?
```javascript
let count = 0
const nums = [0, 1, 2, 3]
nums.forEach(num => {
  if (num) count += 1
})

console.log(count)
```
<details><summary><b>A</b></summary>
<p>
3

nums第一個值為false(0)，故不執行if判斷，所以只執行三次，count為3
</p>
</details>

---

### Front end Q95: 輸出為何?
```javascript
const user = {
  email: "e@mail.com",
  password: "12345"
}

const updateUser = ({ email, password }) => {
  if (email) Object.assign(user, { email })
  if (password) user.password = password

  return user
}

const updateUser = updateUser({ email: "new@mail.com" })
console.log(updateUser === user)
```
<details><summary><b>A</b></summary>
<p>
true

updateUser回傳值為user，updateUser === user = true
</p>
</details>

---

### Front end Q96: 輸出為何?
```javascript
const fruit = ['🍌', '🍊', '🍎']

fruit.slice(0, 1)
fruit.splice(0, 1)
fruit.unshift('🍇')
```
<details><summary><b>A</b></summary>
<p>
['🍇', '🍊', '🍎']

slice: slice會複製一個新陣列，不會改變原始陣列 = ['🍌']

splice: splice會改變原始陣列，splice(start, deleteCount, item) = ['🍊', '🍎']

unshift: unshift會改變原始陣列，在陣列最前面加入一個新元素 = ['🍇', '🍊', '🍎']

</p>
</details>