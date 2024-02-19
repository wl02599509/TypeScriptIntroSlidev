---
theme: dracula
class: text-center
highlighter: shikiji
lineNumbers: false
info: |
  ## Slidev Starter Template
  Presentation slides for developers.

  Learn more at [Sli.dev](https://sli.dev)
drawings:
  persist: false
transition: slide-left
title: Welcome to Slidev
mdc: true
---

# TypeScript 基本介紹

<div class="pt-12">
  <span @click="$slidev.nav.next" class="px-2 py-1 rounded cursor-pointer" hover="bg-white bg-opacity-10">
    進入正題 <carbon:arrow-right class="inline"/>
  </span>
</div>

<div class="abs-br m-6 flex gap-2">
  <button @click="$slidev.nav.openInEditor()" title="Open in Editor" class="text-xl slidev-icon-btn opacity-50 !border-none !hover:text-white">
    <carbon:edit />
  </button>
  <a href="https://github.com/slidevjs/slidev" target="_blank" alt="GitHub" title="Open in GitHub"
    class="text-xl slidev-icon-btn opacity-50 !border-none !hover:text-white">
    <carbon-logo-github />
  </a>
</div>

---

# 目標

-  **TS 與 JS 的差異**
- **資料型態宣告**
- **斷言**
- **Function**
- **物件**

<style>
h1 {
  background-color: #2B90B6;
  background-image: linear-gradient(45deg, #4EC5D4 10%, #146b8c 20%);
  background-size: 100%;
  -webkit-background-clip: text;
  -moz-background-clip: text;
  -webkit-text-fill-color: transparent;
  -moz-text-fill-color: transparent;
}
</style>

---

# TS 與 JS 的差異

- **JS 為語言、TS 為 JS 的延伸工具**
- **弱型別**
  ```js
  const plusOne = (number) => {
    return number + 1
  }
  plusOne(5) // 6
  plusOne('5') // '51'
  ```

- **宣告型別**
  ```ts
  const plusOne = (number:number) => {
    return number + 1
  }
  plusOne(5) // 6
  plusOne('5') // Argument of type 'string' is not assignable to parameter of type 'number'.
  ```

- **編譯階段發現問題**
  ```
  plus.ts:6:10 - error TS2345: Argument of type 'string' is not assignable to parameter of type 'number'.
  ```

---

# 宣告型態 - 變數

<v-click>
```ts
const age: number = 28
const name: string = 'Mike'
const married: boolean = false
const songList: string[] = ['披星戴月的想你', '你比從前快樂', '十年']
const singers: Array<string>: ['JJ', '王力宏', '羅志祥']
const deposit: null = null
const wallet: undefined = undefined
const sport: any
```
</v-click>
<v-click depth='2'>
<span class='text-red-500'>給錯型態時:</span>

```ts
const age:number = '28'

Type 'string' is not assignable to type 'number'.
```
</v-click>

---

# 宣告型態 - Function

<v-click>
定義 Function
```ts
const playVideo = (name: string, speed: number) => {
  console.log(`以 ${speed} 倍速播放 ${name}`)
}
```
</v-click>

<v-click depth='2'>
```ts
playVideo('幽游白書', 1)
```
</v-click>

<v-click depth='3'>
```ts
const name: string = '幽游白書'
const speed: number = 3

playVideo(name, speed)
```
</v-click>

<v-click depth='4'>
<span class='text-red-500'>給錯型態時:</span>
```ts
playVideo(123, 3)

類型 'number' 的引數不可指派給類型 'string' 的參數。
```
</v-click>

---

# 宣告型態 - 物件

<div class='flex gap-x-10'>
  <v-click depth='1'>
    <div>
定義物件
```ts
const anneHathaway = {
  age: 41,
  movies: ['穿著Prada的惡魔', '高年級實習生', '星際效應'],
  married: true
}

```
    </div>
  </v-click>

  <v-click depth='2'>
    <div>
定義介面 (interface) 或 型態 (type)
      <div class='flex gap-x-4'>
        <div>
```ts
interface Actress {
  age: number
  movies: string[]
  married: boolean
}
```
        </div>
        <div>
```ts
type Actress = {
  age: number,
  movies: string[],
  married: boolean
}
```
        </div>
      </div>
    </div>
  </v-click>
</div>

<v-click depth='4'>
  <span>給予物件型態</span>
  <div class='flex gap-x-10'>
    <div>

```ts
const anneHathaway: Actress = {
  age: 41,
  movies: ['穿著Prada的惡魔', '高年級實習生', '星際效應'],
  married: false
}
```
    </div>
  </div>
</v-click>

---

# 宣告型態 - 物件

<span class='text-red-500'>給錯型態時:</span>

<v-click>
  <div>
  <span>1. 少了 age 屬性:</span>
```ts
const anneHathaway2: Actress = {
  movies: ['穿著Prada的惡魔', '高年級實習生', '星際效應'],
  married: false
}

類型 '{ movies: string[]; married: false; }' 缺少屬性 'age'，但類型 'Actress' 必須有該屬性。
```
  </div>
</v-click>

<v-click depth='2'>
  <div>
  <span>2. movie 型態錯誤:</span>
```ts
const anneHathaway2: Actress = {
  age: 41,
  movies: [123, '高年級實習生', '星際效應'],
  married: false
}

類型 'number' 不可指派給類型 'string'。
```
  </div>
</v-click>

---

# 泛型

```ts
function createArray<T>(length: number, value: T): Array<T> {
    let result: T[] = [];
    for (let i = 0; i < length; i++) {
        result[i] = value;
    }
    return result;
}

createArray<string>(3, 'x'); // ['x', 'x', 'x']
```
<v-click dept='2'>
```ts
interface CreateArrayFunc<T> {
    (length: number, value: T): Array<T>;
}

let createArray: CreateArrayFunc<any>;
createArray = function<T>(length: number, value: T): Array<T> {
    let result: T[] = [];
    for (let i = 0; i < length; i++) {
        result[i] = value;
    }
    return result;
}

createArray(3, 'x'); // ['x', 'x', 'x']
```
</v-click>
