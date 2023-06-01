# Array(m) 和 new Array(m) 的差別



## Array(m)

	- 這種方式使用全域的 `Array` 建構函式來創建陣列
	- 它會創建一個包含 `m` 個元素的新陣列，並將每個元素初始化為預設值（通常是 `undefined`）
	- 沒有使用 `new` 關鍵字，直接呼叫 `Array(m)`


## new Array(m) 

	- 這種方式使用 `Array` 建構函式作為物件的建構函式來創建陣列
	- 它也會創建一個包含 `m` 個元素的新陣列，並將每個元素初始化為預設值（通常是 `undefined`）
	- 使用 `new` 關鍵字，呼叫 `new Array(m)`


在大多數情況下，`Array(m)` 和 `new Array(m)` 是等價的，都可以用來創建一個指定長度的新陣列

```js
const arr1 = Array(5);
console.log(arr1); // 輸出：[undefined, undefined, undefined, undefined, undefined]

const arr2 = new Array(5);
console.log(arr2); // 輸出：[undefined, undefined, undefined, undefined, undefined]

```