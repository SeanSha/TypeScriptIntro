# TypeScript 基础：一步一步跟着做

这份文档对着仓库 [TalhaBedirGU/TypeScriptIntro](https://github.com/TalhaBedirGU/TypeScriptIntro) 里的课例来写。每一课都是同一件事：打开哪个文件、运行哪条命令、屏幕上应该出现什么、你要改哪一行、改完怎样才算学会。

先把第 0 步到第 5 课做完。那是 TypeScript 基础。第 6 课用这些基础去跑一个小对话程序，用到了 XState，可以放到基础做完之后。

## 学习顺序

| 顺序 | 文件 | 这一课你要会的东西 |
|---|---|---|
| 0 | 无 | 进入目录，用 `npx tsx` 跑一个 `.ts` 文件 |
| 1 | `variable-types-unions-optionals.ts` | 类型、类型错误、联合类型、`?` |
| 2 | `objects-let-const.ts` | 对象，`const` 和 `let` 的差别 |
| 3 | `ternary-operator.ts` | 三元运算符 `条件 ? A : B` |
| 4 | `logical-operators.ts` | `\|\|`、`&&`、`??` |
| 5 | `lambda-arrays-objects.ts` | 箭头函数、数组方法、展开对象 |
| 6 | `library-agent-machine.ts`、`library-agent-readline.ts` | 把上面的写法用进一个小程序 |

每一课都在这个仓库的根目录里做，不要去改别的文件夹。

---

## 第 0 步：准备好，并跑通第一个文件

**要做什么。** 确认 Node 能用，然后从终端跑起第 1 课的文件。

**为什么。** TypeScript 文件不能直接用 `node 文件.ts` 跑。这个仓库没有自己的编译配置，用 `npx tsx` 会临时下载一个运行器，直接执行 `.ts`。

**怎么做。**

1. 打开终端。在 Cursor 里可以用 Terminal → New Terminal。
2. 进入这个仓库：

```bash
cd /Users/sean/Documents/Coding/GU/Dialogsystem2/TypeScriptIntro
```

如果你的电脑上路径不一样，就 `cd` 到含有这些 `.ts` 文件的那个 `TypeScriptIntro` 文件夹。确认你在对的地方：

```bash
pwd
ls
```

`ls` 里应该能看到 `variable-types-unions-optionals.ts`、`objects-let-const.ts` 这些文件。

3. 看 Node 版本：

```bash
node -v
```

需要 Node 18 或更新。这台机器上是 v23，可以直接用。

4. 跑第 1 课：

```bash
npx tsx variable-types-unions-optionals.ts
```

第一次会下载 `tsx`，等它结束。成功时，终端前几行是：

```text
Ada
24
true
[ 'JavaScript', 'HTML' ]
Paris 95
```

看到这些字，环境就好了。后面每一课都是同一条命令，只换文件名：

```bash
npx tsx 文件名.ts
```

改完文件再跑一次，才能看到新结果。只保存、不重跑，终端里还是旧输出。

---

## 第 1 课：变量类型、联合类型、可选值

文件：`variable-types-unions-optionals.ts`

**要做什么。** 读这个文件，先原样跑一遍，再按文件底部的 5 个课堂练习改代码，每改一处就再跑一次。

**为什么。** TypeScript 的核心是：一个名字只能放它声明过的那种值。写错了，编辑器会画红线，程序也可以拒绝编译。这一课把后面会反复用到的四种写法放在一起：显式类型、自动推断、`A | B`、`?`。

### 1.1 先原样运行

```bash
npx tsx variable-types-unions-optionals.ts
```

你应该看到这些结果，按出现顺序：

```text
Ada
24
true
[ 'JavaScript', 'HTML' ]
Paris 95
SK123 12
Grace is 30 years old.
ABC123
987654
scheduled
delayed
ID: PASS-100
ID: 500
Text: HELLO
Number: 42.00
{ name: 'Lin', age: 28, passportNumber: 'P123456' }
{ name: 'Sam', age: 35, passportNumber: 'P987654', email: 'sam@example.com', mealPreference: 'vegetarian' }
No email was provided.
No email provided
Hello Maya.
Hello Maya. Your destination is Berlin.
100
80
100
undefined
A12
--- Booking Summary ---
ID: BOOK-101
Passenger: Nora
Destination: Rome
Status: scheduled
Seat: Not assigned
--- Booking Summary ---
ID: BOOK-101
Passenger: Nora
Destination: Rome
Status: delayed
Seat: 14C
```

输出对上了，再往下改。对不上，先确认终端当前目录是 `TypeScriptIntro`。

### 1.2 看懂文件里的四块

**类型注解。** 冒号后面是这个变量允许的类型。

```typescript
let studentName: string = "Ada";
let studentAge: number = 24;
let isEnrolled: boolean = true;
let completedCourses: string[] = ["JavaScript", "HTML"];
```

`string` 是文字，`number` 是数字，`boolean` 是 `true` 或 `false`，`string[]` 是一串文字。

**推断。** 右边已经能看出类型时，可以不写冒号。`let city = "Stockholm"` 会被看成 `string`，所以后面 `city = "Paris"` 可以，`city = 3` 不行。

**联合类型。** 竖线表示“可以是其中一种”。

```typescript
let bookingReference: string | number = "ABC123";
bookingReference = 987654;
```

文字和数字都可以。`true` 不行，因为 `boolean` 不在这个联合里。

字面量联合更窄，只允许列出的那几个字：

```typescript
type FlightStatus = "scheduled" | "delayed" | "cancelled";
```

`type` 是给一个类型起名字，后面可以反复用。

**收窄。** 一个值可能是文字也可能是数字时，不能直接当文字用。先用 `typeof` 分开：

```typescript
if (typeof value === "string") {
  return `Text: ${value.toUpperCase()}`;
}
return `Number: ${value.toFixed(2)}`;
```

进了 `if`，TypeScript 知道这里一定是 `string`，才允许 `.toUpperCase()`。

**可选。** `?` 只用在对象属性和函数参数上，表示“可以不给”。不给的时候，值是 `undefined`。

```typescript
email?: string;
function greetPassenger(name: string, destination?: string): string {
```

变量本身不能写成 `let gate?: string`。变量若可以没有值，写成 `string | undefined`。

缺了的值用 `??` 补一个默认：

```typescript
const seat: string = currentBooking.seatNumber ?? "Not assigned";
```

只有左边是 `null` 或 `undefined` 时才用右边。`0` 和 `""` 会保留。第 4 课专门比较这个和 `||`。

### 1.3 故意制造一个类型错误

文件里有几行被注释掉了，例如：

```typescript
// flightNumber = 123;
```

**怎么做。**

1. 删掉这一行前面的 `//`，保存。
2. 看编辑器红线。把鼠标放上去，读英文错误。这里会说 number 不能赋给 string。
3. 再跑：

```bash
npx tsx variable-types-unions-optionals.ts
```

`tsx` 默认仍会跑，红线主要在编辑器里看。以编辑器的报错为准。
4. 把 `//` 加回去，或者把 `123` 改成 `"123"`，红线消失。

同样可以试这些被注释的行，一次只打开一行：

- `availableSeats = "twelve"`
- `isEnrolled = "yes"`
- `completedCourses.push(42)`
- `printPassenger(30, "Grace")`
- `bookingReference = true`
- `currentStatus = "waiting"`

每打开一行，读完错误，再注释回去。这一步的目的是学会读错误，不是把文件改坏。

### 1.4 做文件底部的 5 个练习

练习写在文件最下面的 `CLASSROOM EXERCISES`。做完再对照下面的写法。

**练习 1。** 一个变量，既可以是文字，也可以是布尔值。

加在文件末尾：

```typescript
let answer: string | boolean = "yes";
answer = false;
console.log(answer);
```

再跑，最后一行应是 `false`。如果写成 `answer = 1`，编辑器会报错，因为 `number` 不在联合里。

**练习 2。** 给 `FlightStatus` 加上 `"boarding"`。

找到这一行：

```typescript
type FlightStatus = "scheduled" | "delayed" | "cancelled";
```

改成：

```typescript
type FlightStatus = "scheduled" | "delayed" | "cancelled" | "boarding";
```

然后可以把后面的 `currentStatus = "delayed"` 暂时改成 `currentStatus = "boarding"`，跑一次，输出里会出现 `boarding`。改完记着改回去，或者留着也行，只要类型里有这个词。

**练习 3。** 给 `Passenger` 加一个可选电话。

在 `mealPreference` 那一行下面加：

```typescript
phoneNumber?: string;
```

可选的意思是：`passengerOne` 可以继续不写电话。你可以只给 `passengerTwo` 加 `phoneNumber: "0700000000"`。不写这个字段的对象仍然合法。

**练习 4。** 写一个函数：一个必填参数，一个可选参数。

```typescript
function callPassenger(name: string, phoneNumber?: string): string {
  if (phoneNumber !== undefined) {
    return `Calling ${name} at ${phoneNumber}.`;
  }
  return `No phone number for ${name}.`;
}

console.log(callPassenger("Lin"));
console.log(callPassenger("Sam", "0700000000"));
```

输出应是：

```text
No phone number for Lin.
Calling Sam at 0700000000.
```

**练习 5。** 就是 1.3。打开一处错误，读懂，再改对。做完把演示用的错误行恢复成注释。

### 1.5 这一课过关的标准

你能不看文档写出这四行，并且知道每一行为什么这样写：

```typescript
let city: string = "Gothenburg";
let code: string | number = 12;
type Seat = { row: number; letter?: string };
function label(name: string, title?: string): string {
  return title ?? name;
}
```

---

## 第 2 课：对象、`const` 和 `let`

文件：`objects-let-const.ts`

**要做什么。** 原样跑一遍，再完成文件底部的小练习。

**为什么。** 对话系统里的乘客、航班、预订都是对象：若干字段放在一个名字下面。`const` 锁住的是“这个名字不能改去指向另一个对象”，不是“里面的字段不能改”。

### 2.1 运行

```bash
npx tsx objects-let-const.ts
```

应看到：

```text
Original passenger: { name: 'Ada', age: 28, hasCheckedIn: false }
Updated passenger: { name: 'Ada', age: 29, hasCheckedIn: true }
First flight: { flightNumber: 'SK123', destination: 'Paris', delayed: false }
Delayed flight: { flightNumber: 'SK123', destination: 'Paris', delayed: true }
New current flight: { flightNumber: 'SK456', destination: 'London', delayed: false }
Booking: { reference: 'ABC123', destination: 'Rome', confirmed: true }
```

### 2.2 对照代码看两件事

`passenger` 用 `const`。这两行可以，因为改的是字段：

```typescript
passenger.hasCheckedIn = true;
passenger.age = 29;
```

文件里注释掉的整段重新赋值不行。你可以自己试一次：把注释打开，看红线，再注释回去。错误的意思是不能给一个 `const` 变量重新赋值。

`currentFlight` 用 `let`。所以既可以改 `delayed`，也可以后来整个换成去伦敦的那一班。换上去的新对象必须还有 `flightNumber`、`destination`、`delayed`，而且类型要对：航班号是文字，`delayed` 是布尔值。

`type Booking` 是把对象的形状写死。少一个字段，或者把 `confirmed: false` 写成 `confirmed: "yes"`，编辑器会报错。

记这一句：

- `const` 对象：字段能改，整个对象不能换。
- `let` 对象：字段能改，整个对象也能换，但新对象要符合原来的形状。

### 2.3 做文件底部的练习

在文件末尾加上这些，然后重跑。

```typescript
const airport = {
  name: "Arlanda",
  city: "Stockholm",
  isOpen: true,
};

airport.isOpen = false;
console.log("Airport:", airport);

let selectedGate = {
  code: "A12",
  open: true,
};

selectedGate = {
  code: "B4",
  open: false,
};

console.log("Gate:", selectedGate);
```

应多出：

```text
Airport: { name: 'Arlanda', city: 'Stockholm', isOpen: false }
Gate: { code: 'B4', open: false }
```

再试一行错误练习：在 `airport.isOpen = false` 下面写 `airport = { name: "Bromma", city: "Stockholm", isOpen: true }`。应出现不能给 `const` 赋值的红线。看完删掉这行。

---

## 第 3 课：三元运算符

文件：`ternary-operator.ts`

**要做什么。** 跑一遍，然后自己写两个三元表达式：一个得到文字，一个得到数字。

**为什么。** 很多时候你不是要执行一大段语句，只是要在两个值里挑一个。三元运算符就是这个选择，能直接放进赋值和模板字符串。

形状永远是：

```typescript
条件 ? 条件为真时的值 : 条件为假时的值
```

问号前面是条件，冒号两边是两个结果。两个结果会成为整个表达式的值。

### 3.1 运行

```bash
npx tsx ternary-operator.ts
```

应看到：

```text
You are an adult.
Access granted
Access granted
Flight status: Delayed
Ada: Not checked in
It is warm.
It is cool.
Bonus points: 10
Passed: true
Passed (simplified): true
You may board.
Bienvenue à Paris!
```

### 3.2 和 if/else 对上

文件里同一件事写了两遍。`if/else` 要先声明一个 `let`，再在两个分支里赋值。三元表达式一行得到同一个文字。

当每个分支里要做很多件事（好几次 `console.log`、改好几个字段）时，用 `if/else`。只是挑一个值时，用三元。

不要写成三层嵌套的三元。三个以上的结果，用 `if/else`，或者用联合类型配合 `if`。

布尔值不需要 `条件 ? true : false`。`score >= 50` 本身就是 `boolean`。文件里 `passedSimplified` 就是在演示这一点。

### 3.3 自己写

在文件末尾加：

```typescript
const temperature: number = 3;
const weather: string = temperature < 0 ? "freezing" : "not freezing";
const extraFee: number = temperature < 0 ? 15 : 0;

console.log(weather);
console.log(extraFee);
```

`temperature` 是 `3` 时，输出：

```text
not freezing
0
```

把 `3` 改成 `-2`，再跑，应变成：

```text
freezing
15
```

---

## 第 4 课：`||`、`&&`、`??`

文件：`logical-operators.ts`

**要做什么。** 跑一遍，对照每一节的注释，最后自己用三个运算符各写一个函数。

**为什么。** 可选字段经常是 `undefined`。你要决定：什么时候改用默认值。选错运算符，用户真的填了 `0`、空字符串或 `false` 时，会被你换成默认值。

### 4.1 运行

```bash
npx tsx logical-operators.ts
```

应看到（空行也算，空字符串打印出来就是空行）：

```text
--- 1. Logical OR: || ---
Guest
Turkish
Window seat

--- 2. Be careful with valid falsy values ---
50
Default message

--- 3. Logical AND: && ---
true
false
Passenger may board.
Welcome back!

--- 4. Nullish coalescing: ?? ---
Gate not assigned
0

false

--- 5. Comparing || and ?? ---
3
0
Untitled

--- 6. Optional object properties ---
light
0
false

--- 7. Useful functions ---
Anonymous
0
false

--- 8. Combining operators safely ---
Unknown status
true
```

第 5 节里 `optionalTitle ?? "Untitled"` 的结果是空字符串，所以 `Untitled` 那一行和下一节标题之间有一个空行。那不是漏打印，空字符串就是结果。

### 4.2 三个运算符分别做什么

假值（falsy）有：`false`、`0`、`""`、`null`、`undefined`、`NaN`。

| 写法 | 什么时候用右边 | 这一课里的例子 |
|---|---|---|
| `value \|\| fallback` | 左边是任意假值 | `"" \|\| "Guest"` 得到 `"Guest"`；`0 \|\| 50` 得到 `50` |
| `value ?? fallback` | 左边只有 `null` 或 `undefined` | `0 ?? 50` 得到 `0`；`undefined ?? "Gate not assigned"` 得到默认文字 |
| `a && b` | 左边为真时，结果是右边；左边为假时，结果是左边 | `true && "Welcome back!"` 得到那段文字；`false && ...` 不会去用右边 |

选择口诀，文件末尾也写了：如果 `0`、`""`、`false` 是有效数据，用 `??`。如果这些也应该当成“没有填”，用 `||`。

音量 `0` 是有效的，所以 `savedVolume || 50` 错了，会变成 `50`。`savedVolume ?? 50` 才保留 `0`。

名字是空字符串时通常想显示客人，所以 `name || "Anonymous"` 是合理的。

`&&` 不只是“两个都要为真”。它还会返回其中一个操作数。要保证得到真正的布尔值，写成 `Boolean(token)`，或者写成 `token !== undefined`。

`??` 不能和 `||`、`&&` 直接挤在同一个表达式里，必须加括号：

```typescript
const status: string = (customStatus ?? "") || "Unknown status";
```

### 4.3 自己写三个函数

加在文件末尾：

```typescript
const showName = (name: string | undefined): string => name || "Guest";

const showVolume = (volume: number | undefined): number => volume ?? 30;

const canDepart = (hasTicket: boolean, gateOpen: boolean): boolean =>
  hasTicket && gateOpen;

console.log(showName(""));
console.log(showVolume(0));
console.log(canDepart(true, false));
```

应看到：

```text
Guest
0
false
```

如果 `showVolume(0)` 打印出 `30`，说明你写成了 `||`。改回 `??`。

---

## 第 5 课：箭头函数、数组、对象展开

文件：`lambda-arrays-objects.ts`

**要做什么。** 跑一遍，然后改最后的筛选条件，并自己加一个 `map`。

**为什么。** 后面的代码很少写 `function` 关键字，多数是箭头函数。数组也不会每次都用 `for` 循环改原数组，而是用 `map`、`filter`、`find` 得到一个新数组。

箭头函数的形状：

```typescript
const 名字 = (参数: 类型): 返回类型 => 表达式或代码块;
```

只有一个表达式时，不用写 `return`，那个表达式就是返回值。有多行时，加大括号，并且自己写 `return`。没有有用的返回值时，返回类型写 `void`。

### 5.1 运行

```bash
npx tsx lambda-arrays-objects.ts
```

应看到：

```text
Welcome to the flight desk!
Hello, Ada!
145
80 EUR is cheap.
STOCKHOLM
[
  'Flight to Stockholm',
  'Flight to Paris',
  'Flight to Rome',
  'Flight to Berlin'
]
[ 'Paris', 'Rome' ]
Paris
Rome available: true
[ 'Stockholm', 'Paris', 'Rome', 'Berlin', 'Madrid' ]
{ id: 1, destination: 'Paris', price: 100, available: true }
Paris: 100 EUR
[ 'Paris', 'Berlin' ]
Total price: 360 EUR
1: Paris
2: Rome
3: Berlin
[
  { id: 1, destination: 'Paris', price: 120, available: true },
  { id: 2, destination: 'Rome', price: 90, available: false }
]
```

### 5.2 把数组方法对上输出

`cities` 是 `["Stockholm", "Paris", "Rome", "Berlin"]`。

| 方法 | 作用 | 这个文件里的结果 |
|---|---|---|
| `map` | 每一项都变成一个新值，长度不变 | 每项前面加上 `Flight to ` |
| `filter` | 留下条件为真的项 | 名字长度不超过 5 的：`Paris`、`Rome` |
| `find` | 第一个匹配的项；没有就是 `undefined` | `"Paris"` |
| `some` | 有没有至少一项匹配，结果是布尔值 | `true` |
| `forEach` | 对每一项做一件事，不返回新数组 | 打印每个航班的 id 和目的地 |
| `reduce` | 把所有项收成一个值 | 三个价格相加是 `360` |

`find` 可能找不到，所以类型是 `string | undefined`。文件里用 `paris ?? "City not found"` 处理没有结果的情况。

展开数组不会改原数组：

```typescript
const moreCities: string[] = [...cities, "Madrid"];
```

`cities` 还是 4 个城市。`moreCities` 是 5 个。

对象也一样。`{ ...parisFlight, price: 100 }` 复制一份，只把价格改成 100。原来的 `flights[0]` 仍是 120。后面筛选“低于 130”时，Paris 仍然用 120，所以它还在结果里。这是这一课最容易看错的地方。

解构是把字段取出来：

```typescript
const { destination, price } = discountedFlight;
```

### 5.3 改最后的练习

文件最后已经是：

```typescript
const affordableFlights: Flight[] = flights.filter((flight) => flight.price < 130);
```

这会留下 Paris（120）和 Rome（90），Berlin（150）被去掉。输出里的最后一段就是这个结果。

把 `130` 改成 `100`，保存，再跑。只应剩下 Rome，因为 90 小于 100，120 和 150 都不是。

改回去之前，再加一个自己的 `map`。放在 `affordableFlights` 后面：

```typescript
const affordableLabels: string[] = affordableFlights.map(
  (flight) => `${flight.destination} costs ${flight.price}`
);
console.log(affordableLabels);
```

`130` 还没改回去时，应看到：

```text
[ 'Rome costs 90' ]
```

改回 `130` 后再跑，应看到 Paris 和 Rome 两句。

---

## 第 6 课：用这些基础读一个小程序

这一课不是新的语法课。它把前面的类型、可选值、箭头函数和数组查找放进两个文件。先做基础的五课，再看这里。

两个文件讲的是同一个图书馆柜台：

- `library-agent-machine.ts`：程序自己说出四句话，把每条路走一遍。适合先读。
- `library-agent-readline.ts`：你在终端里输入书名。

它们都依赖 `xstate`。这个仓库一开始没有 `package.json`，所以要先安装。

**怎么做。** 在 `TypeScriptIntro` 目录里：

```bash
npm install xstate
npx tsx library-agent-machine.ts
```

`npm install` 会生成 `node_modules/`、`package.json`、`package-lock.json`。`node_modules/` 不要提交。

应看到类似：

```text
LIBRARIAN: Enter a book title, or say "That's all".

USER: Dune
LIBRARIAN: "Dune" exists, but is unavailable.
LIBRARIAN: Enter a book title, or say "That's all".

USER: The Hobbit
LIBRARIAN: "The Hobbit" is available.
LIBRARIAN: Enter a book title, or say "That's all".

USER: Harry Potter
LIBRARIAN: I could not find "Harry Potter".
LIBRARIAN: Enter a book title, or say "That's all".

USER: That's all
LIBRARIAN: Goodbye!
```

### 6.1 先认你已经学过的部分

书的类型就是第 1、2 课的对象类型：

```typescript
type Book = {
  title: string;
  available: boolean;
};
```

`findBook` 是箭头函数。找不到时返回 `undefined`，所以返回类型是 `Book | undefined`。

```typescript
const findBook = (input: string): Book | undefined => {
  return library[normalize(input)];
};
```

`book?.title` 里的 `?.` 叫可选链：左边是 `undefined` 时，整个表达式是 `undefined`，不会因为读取 `.title` 而崩溃。

`findBook(event.text)?.available === true` 用了可选链，再和一个布尔值比较。找不到书时，左边是 `undefined`，比较结果是 `false`。

`normalize` 把输入变成小写并去掉两端空格，所以 `"  Dune  "` 和 `"dune"` 能对上同一本书。

### 6.2 状态机在这个文件里只看四件事

你现在不需要学完整的 XState。读 `library-agent-machine.ts` 时只抓这四块：

1. `guards`：每个守卫是一个返回布尔值的函数，用来判断走哪条路。
2. `actions`：真正打印句子的函数。
3. `states`：程序当前停在哪一步。
4. `on: { USER_INPUT: [ ... ] }`：收到一句话时，从上到下检查守卫，用第一个成立的那条。

顺序是：

1. 用户说 `That's all` → `finished`，打印再见，结束。
2. 书存在并且 `available === true` → 打印可借。
3. 书存在但不可借 → 打印不可借。
4. 上面都不成立 → 没找到。

`Dune` 在这个文件里是 `available: false`，所以走“存在但不可借”。`The Hobbit` 可借。`Harry Potter` 不在表里。

### 6.3 自己改一条数据，预测输出

在 `library-agent-machine.ts` 里把 Dune 改成可借：

```typescript
dune: { title: "Dune", available: true },
```

保存后再跑。`"Dune" exists, but is unavailable.` 应变成 `"Dune" is available.`。预测对了，说明你读懂了守卫的顺序。看完可以把 `available` 改回 `false`。

### 6.4 再跑交互版

```bash
npx tsx library-agent-readline.ts
```

程序会停住等你打字。这个文件里的书表和上一个文件不一样：

| 书名 | 这一文件里能否借 |
|---|---|
| The Hobbit | 可以 |
| 1984 | 不可以 |
| Dune | 可以 |

依次输入：

```text
Dune
1984
Harry Potter
That's all
```

你应看到 Dune 可借，1984 在馆但不可借，Harry Potter 找不到，然后程序说再见并结束。输入时大小写和两端空格不影响，因为查找前做了 `toLowerCase()` 和 `trim()`。

不想继续时输入 `That's all`。大小写可以不同。

---

## 做完之后怎么检查

基础部分不要看文档，在一个新文件 `check.ts` 里自己写，然后 `npx tsx check.ts`。

1. 一个 `string | number` 变量，先赋文字，再改成数字。
2. 一个 `type Passenger`，有必填 `name: string`，可选 `seat?: string`。
3. 一个 `const` 乘客对象，改它的座位，不要整个替换对象。
4. 一个三元表达式：有座位就返回座位，否则返回 `"unassigned"`。这一项用 `seat ?? "unassigned"` 更合适，两种都写一次。
5. 一个数组 `filter`，只留下票价小于 `100` 的航班；再用 `map` 取出目的地。

参考写法：

```typescript
let id: string | number = "AB1";
id = 7;

type Passenger = {
  name: string;
  seat?: string;
};

const passenger: Passenger = { name: "Nora" };
passenger.seat = "14C";

const seatLabel: string = passenger.seat !== undefined ? passenger.seat : "unassigned";
const seatLabelWithNullish: string = passenger.seat ?? "unassigned";

type Flight = { destination: string; price: number };
const flights: Flight[] = [
  { destination: "Paris", price: 120 },
  { destination: "Rome", price: 90 },
];

const cheapDestinations: string[] = flights
  .filter((flight) => flight.price < 100)
  .map((flight) => flight.destination);

console.log(id);
console.log(seatLabel);
console.log(seatLabelWithNullish);
console.log(cheapDestinations);
```

输出应是：

```text
7
14C
14C
[ 'Rome' ]
```

这五行和输出都对上，TypeScript 基础这一遍就做完了。

`check.ts` 是你自己的练习，可以留在本机，不必提交。
