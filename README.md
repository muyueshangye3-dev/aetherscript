変数と型
let で変数を宣言します。: で型を指定できます（省略可能）。
let name = "Alice"
let age: number = 30
let active: boolean = true
let score = null

print(name)    // Alice
print(age)     // 30
テンプレートリテラル
バッククォートと ${} で変数を文字列に埋め込めます。
let name = "AetherScript"
let ver = 1

print(`言語名: ${name}`)
print(`バージョン: ${ver}`)
// 計算も埋め込める
print(`1 + 2 = ${1 + 2}`)
演算子
算術・比較・論理・複合代入・インクリメントが使えます。
let x = 10
x += 5     // 15
x -= 3     // 12
x *= 2     // 24
x /= 4     // 6
x++        // 7

print(x > 5 && x < 10)  // true
print(x == 7 || x == 8) // true
print(!false)            // true
if / else if / else
条件に応じて処理を分岐します。
let score = 85

if (score >= 90) {
  print("A")
} else if (score >= 70) {
  print("B")
} else {
  print("C")
}
// → B
while / for
繰り返し処理。for は C スタイルと for...of が使えます。
// while
let i = 0
while (i < 3) {
  print(i)
  i++
}

// for（C スタイル）
for (let j = 0; j < 3; j++) {
  print(j)
}

// for...of（配列・文字列）
for (let x of [10, 20, 30]) {
  print(x)
}
switch / break / continue
switch で多分岐。break でループ脱出、continue でスキップ。
switch (2) {
  case 1: print("one") break
  case 2: print("two") break
  default: print("other")
}
// → two

for (let i = 1; i <= 5; i++) {
  if (i == 3) { continue }
  if (i == 5) { break }
  print(i)   // 1 2 4
}
try / catch / throw
エラー処理。throw で例外を投げ、catch で受け取ります。
fn divide(a, b) {
  if (b == 0) {
    throw "ゼロ除算エラー"
  }
  return a / b
}

try {
  let r = divide(10, 0)
} catch (e) {
  print(e)  // ゼロ除算エラー
}
関数の定義
fn で関数を定義します。引数と戻り値に型を指定できます。
fn add(a: number, b: number): number {
  return a + b
}

print(add(3, 4))   // 7

// 無名関数
let double = fn(x) { return x * 2 }
print(double(5))   // 10
再帰・クロージャ
関数は自分自身を呼べます。外側の変数を閉じ込めるクロージャも使えます。
// 再帰（フィボナッチ）
fn fib(n) {
  if (n <= 1) { return n }
  return fib(n - 1) + fib(n - 2)
}
print(fib(10))  // 55

// クロージャ（カウンター）
fn makeCounter() {
  let count = 0
  return fn() { count++
return count }
}
let c = makeCounter()
print(c())  // 1
print(c())  // 2
デフォルト引数
引数が渡されなかった場合のデフォルト値を設定できます。
fn greet(name, prefix) {
  if (prefix == null) { prefix = "Hello" }
  return `${prefix}, ${name}!`
}

print(greet("Alice", null))
// → Hello, Alice!
print(greet("Bob", "Hi"))
// → Hi, Bob!
配列
[] で配列を作ります。インデックスでアクセス、分割代入も使えます。
let arr = [10, 20, 30]
print(arr[0])   // 10

// 分割代入
let [a, b, c] = arr
print(b)        // 20

// array モジュール
import array
array.push(arr, 40)
print(array.length(arr))  // 4
print(array.join(arr, "-"))  // 10-20-30-40
オブジェクト
{} でオブジェクトを作ります。スプレッド演算子でコピー・マージができます。
let user = { name: "Alice", age: 30 }
print(user.name)   // Alice

// スプレッドでコピー＆更新
let updated = { ...user, age: 31 }
print(updated.age)  // 31
print(user.age)     // 30（元は変わらない）
Flow の基本
flow キーワードでリアクティブストリームを宣言します。interval で定期的に値を流します。
// 1秒ごとに値を流す
flow tick = interval(1000)

// 値を変換（map）
flow count = tick
  .map(fn(v) { return 1 })
  .scan(fn(a, b) { return a + b }, 0)

// 受け取る（subscribe）
count.subscribe(fn(n) {
  print(`カウント: ${n}`)
})
Flow のフィルタ・スキャン
filter で条件を満たす値のみ通過。scan で状態を累積します。
flow sensor = interval(500)

// 偶数のみ通過
flow evens = sensor
  .map(fn(v) { return 1 })
  .scan(fn(a, b) { return a + b }, 0)
  .filter(fn(n) { return n == n / 2 * 2 })

evens.subscribe(fn(n) {
  print(`偶数: ${n}`)
})
Promise と Flow の連携
fromPromise で Promise を Flow に変換できます。
import time

// sleep 完了後に Flow へ変換
flow result = fromPromise(time.sleep(1000))

result.subscribe(fn(v) {
  print("1秒後に実行されました")
})

// http.getFlow で HTTP レスポンスを Flow に
import http
flow data = http.getFlow("https://api.example.com")
data.subscribe(fn(v) { print(v) })
math モジュール
数学関数を提供します。import "@math" で読み込みます。
import "@math"

print(math.sqrt(16))      // 4
print(math.floor(3.7))    // 3
print(math.ceil(3.2))     // 4
print(math.pow(2, 8))     // 256
print(math.max(10, 20))   // 20
print(math.randInt(1, 6)) // サイコロ
string モジュール
文字列操作の関数群です。
import string

let s = "Hello, World!"
print(string.upper(s))          // HELLO, WORLD!
print(string.lower(s))          // hello, world!
print(string.length(s))         // 13
print(string.includes(s, "World")) // true
print(string.replace(s, "World", "AetherScript"))
// Hello, AetherScript!
fs / http モジュール
ファイル読み書きと HTTP リクエストができます。
import fs
import http

// ファイル書き込み・読み込み
fs.write("/tmp/note.txt", "メモの内容")
let content = fs.read("/tmp/note.txt")
print(content)

// HTTP GET（Promise）
let data = http.get("https://api.example.com")
print(data)

// HTTP → Flow
flow stream = http.getFlow("https://api.example.com")
stream.subscribe(fn(v) { print(v) })
