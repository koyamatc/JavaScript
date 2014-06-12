---
title: JavaScript
layout: post
postTitle: Iterator & Generator
categories: basics
---

## Iterator

イテレータは、一連の処理の中で、集合体のアイテム一つずつにアクセスする方法を知っているオブジェクトです。　　

Javascriptにおいては、一連の処理中の次のアイテムを返す next()メソッドを提供します。
このメソッドは一連の処理が終了した場合に、任意で StopIteration 例外を発生させることができます。

イテレータオブジェクトは作成されると、next() を繰り返し呼び出すことで明示的に、または JavaScript の for...in および for each コンストラクタで暗黙的に使用されることができます。
  
{% highlight javascript %}
// 単純なイテレータは、Iterator() 関数を用いて作成できます
var lang = { name: 'JavaScript', birthYear: 1995 };
var it = Iterator(lang);

/* 初期化後は、オブジェクトにあるキーと値のペアへ次々にアクセスするために next() メソッドを呼び出すことができます */
var pair = it.next(); // ペアは ["name", "JavaScript"]
pair = it.next(); // ペアは ["birthYear", 1995]
pair = it.next(); // StopIteration 例外が発生
{% endhighlight %}

{% highlight javascript %}
/* next() メソッドを直接呼び出す代わりに、for...in ループを使用できます。
ループは StopIteration 例外が発生したときに、自動的に終了します。 */

var it = Iterator(lang);
for (var pair in it)
  print(pair); // 各々の [key, value] のペアを次々に表示

/* オブジェクトのキーに対してのみ反復処理を行いたい場合は、Iterator() 関数の第 2 引数に true を渡します */

var it = Iterator(lang, true);
for (var key in it)
  print(key); // 各々の key を次々に表示
{% endhighlight %}

オブジェクトの内容へのアクセスに Iterator() を使用することの利点は、Object.prototype に追加した独自のプロパティが一連の処理に含まれないことです。

### Iterator() は、配列にも同様に使用できます:
{% highlight javascript %}
var langs = ['JavaScript', 'Python', 'C++'];
var it = Iterator(langs);
for (var pair in it)
  print(pair); // 各々の [index, language] のペアを次々に表示
{% endhighlight %}

オブジェクトと同様に、第 2 引数に true を渡すと配列のインデックスへ反復処理を行うようになります:
{% highlight javascript %}
var langs = ['JavaScript', 'Python', 'C++'];
var it = Iterator(langs, true);
for (var i in it)
  print(i); // 0, 1, 2 と出力
{% endhighlight %}

for ループ内で let キーワードを用いて、ブロックスコープに収めた変数をインデックスと値に割り当てることもでき、インデックスと値を分割して代入できます:
{% highlight javascript %}
var langs = ['JavaScript', 'Python', 'C++'];
var it = Iterator(langs);
for (let [i, lang] in it)
 print(i + ': ' + lang); // "0: JavaScript" などを出力
{% endhighlight %}

### 独自のイテレータの定義
アイテムの集合体を表す一部のオブジェクトは、特定の方法で反復処理を行うべきです。

+ 範囲のオブジェクトの反復処理では、その範囲内の数値を次々に返すべきです。
+ 木構造の葉は、深さ優先または幅優先の探索法でたどることができます。
+ データベースのクエリの出力を表すオブジェクトの反復処理では、出力セットの全体が一つの配列に収められていない場合でも、行を次々に返すべきです。
+ 無限の数列 (フィボナッチ数列など) の反復処理では、無限の長さのデータ構造を作成することなく結果を次々に返せるべきです。

JavaScript では独自の反復処理ロジックのコードを記述し、それをオブジェクトに結びつけることができます。

{% highlight javascript %}
// 下限値と上限値を保管する、シンプルな Range オブジェクトを作成します。
function Range(low, high){
  this.low = low;
  this.high = high;
}
/*
 ここで、上記の範囲に含まれる整数の並びを返す独自のイテレータを作成します。
イテレータのインタフェースは、一連のデータからアイテムを返すか StopIteration 例外を投げる、next() メソッドの提供が必要です。 
*/
function RangeIterator(range){
  this.range = range;
  this.current = this.range.low;
}
RangeIterator.prototype.next = function(){
  if (this.current > this.range.high)
    throw StopIteration;
  else
    return this.current++;
};
/*
RangeIterator は range のインスタンスとともにインスタンス化され、一連のデータ内でどこまで進んだかを追跡するための current プロパティを維持します。
そして、RangeIterator を Range オブジェクトへ関連づけるために、Range へ特別なメソッドである __iterator__ を追加する必要があります。このメソッドは Range のインスタンスで反復処理を行う際に呼び出され、反復処理のロジックを実装する RangeIterator のインスタンスを返します。
*/
Range.prototype.__iterator__ = function(){
  return new RangeIterator(this);
};
// 独自のイテレータでフックすることにより、range のインスタンスでの反復処理を以下のように行うことができます:

var range = new Range(3, 5);
for (var i in range)
  print(i); // 順に 3, 4, 5 を出力
{% endhighlight %}

###ジェネレータ: イテレータ構築のよりよい方法
独自のイテレータは役に立つ手段ですが、内部の状態を明示的に管理する必要があることから、イテレータの作成では注意深くプログラミングすることが求められます。ジェネレータは、強力な代替手段を提供します。これは、自身の状態を管理できる関数一つを記述することにより、反復処理のアルゴリズムの定義を可能にします。

ジェネレータは、イテレータの生成元として働く特別な種類の関数です。1 つ以上の yield 式を含む場合、関数はジェネレータになります。

註: HTML において、yield キーワードは &lt;script type="application/javascript;version=1.7"> (または上位バージョン) のブロックで囲まれたコードブロックでのみ使用可能です。XUL の script タグは、この機能の使用に際して特別なブロックを必要としません。
ジェネレータ関数が関数本体から呼び出されても、すぐには実行されません。代わりに、ジェネレータ・イテレータオブジェクトを返します。各ジェネレータ・イテレータの next() メソッドを呼び出すと、関数の本体を次の yield 式まで実行し、その結果を返します。関数の終端または return 文に達すると、StopIteration 例外が発生します。

この動作は、次の例でよく表現されています:
{% highlight javascript %}
function simpleGenerator(){
  yield "first";
  yield "second";
  yield "third";
  for (var i = 0; i < 3; i++)
    yield i;
}

var g = simpleGenerator();
print(g.next()); // "first" を出力
print(g.next()); // "second" を出力
print(g.next()); // "third" を出力
print(g.next()); // 0 を出力
print(g.next()); // 1 を出力
print(g.next()); // 2 を出力
print(g.next()); // StopIteration が発生
{% endhighlight %}

ジェネレータ関数はクラスの __iterator__ メソッドとして直接使用することができ、独自のイテレータを作成するために必要なコードの量が劇的に減ります。以下は前出の Range を、ジェネレータを使用して書き直したものです:
{% highlight javascript %}
function Range(low, high){
  this.low = low;
  this.high = high;
}
Range.prototype.__iterator__ = function(){
  for (var i = this.low; i <= this.high; i++)
    yield i;
};
var range = new Range(3, 5);
for (var i in range)
  print(i); // 順に 3, 4, 5 を出力
{% endhighlight %}  

すべてのジェネレータが終了するわけではありません。無限の並びを表すジェネレータを作成することができます。次のジェネレータはフィボナッチ数列を実装しており、各要素は自身の前の要素 2 つの値を合計したものです:
{% highlight javascript %}
function fibonacci(){
  var fn1 = 1;
  var fn2 = 1;
  while (1){
    var current = fn2;
    fn2 = fn1;
    fn1 = fn1 + current;
    yield current;
  }
}

var sequence = fibonacci();
print(sequence.next()); // 1
print(sequence.next()); // 1
print(sequence.next()); // 2
print(sequence.next()); // 3
print(sequence.next()); // 5
print(sequence.next()); // 8
print(sequence.next()); // 13
{% endhighlight %}

ジェネレータ関数は引数をとることができ、それは始めに関数が呼び出されるときに結びつけられます。ジェネレータは return 文で (StopIteration 例外を発生させて) 終了することができます。次の fibonacci() は省略可能な limit 引数を持つ派生形で、limit が渡された場合に終了するでしょう。
{% highlight javascript %}
function fibonacci(limit){
  var fn1 = 1;
  var fn2 = 1;
  while (1){
    var current = fn2;
    fn2 = fn1;
    fn1 = fn1 + current;
    if (limit && current > limit)
      return;
    yield current;
  }
}
{% endhighlight %}

###高度なジェネレータ
ジェネレータは生み出す値を要求に応じて計算しており、多くの計算が必要な一連のデータを効率的に表現したり、前出のとおり無限に連なるデータを表現したりすることを可能にします。

ジェネレータ・イテレータオブジェクトは next()

メソッドに加えて、ジェネレータの内部状態を変更するために使用できる send() メソッドも持っています。send() で渡された値は、ジェネレータが最後に立ち止まった yield の結果として扱われます。特定の値を渡すため send() を使用する前に、少なくとも 1 回 next() を呼び出してジェネレータを開始する必要があります。

次の fibonacci ジェネレータは、数列をリスタートするために send() を使用しています:
{% highlight javascript %}
function fibonacci(){
  var fn1 = 1;
  var fn2 = 1;
  while (1){
    var current = fn2;
    fn2 = fn1;
    fn1 = fn1 + current;
    var reset = yield current;
    if (reset){
        fn1 = 1;
        fn2 = 1;
    }
  }
}

var sequence = fibonacci();
print(sequence.next());     // 1
print(sequence.next());     // 1
print(sequence.next());     // 2
print(sequence.next());     // 3
print(sequence.next());     // 5
print(sequence.next());     // 8
print(sequence.next());     // 13
print(sequence.send(true)); // 1
print(sequence.next());     // 1
print(sequence.next());     // 2
print(sequence.next());     // 3
{% endhighlight %}
注意: 興味深い点として、send(undefined) の呼び出しは next() と等価であることがあります。しかし、send() を呼び出す際に undefined 以外の値で新たなジェネレータを開始すると、TypeError 例外が発生するでしょう。

ジェネレータの throw() メソッドを呼び出して発生させる例外を渡すことで、ジェネレータに例外を発生させることができます。この例外はジェネレータが現在止まっているコンテキストから、現在止まっている yield が throw value 文に替わったかのように投げられます。

発生した例外の処理中に yield に出くわさない場合は、例外が throw() の呼び出しを通して伝播し、その後の next() の呼び出しで StopIteration が発生することになります。

ジェネレータは、自身を閉じさせる close() メソッドを持っています。ジェネレータを閉じることの効果は以下のとおりです:

実行中のジェネレータ関数でアクティブなすべての finally 節が実行されます
finally 節で StopIteration 以外の例外が発生すると、その例外は close() メソッドの呼び出し元に伝播します。
ジェネレータが終了します。

###ジェネレータ式
JavaScript 1.8 で導入
配列内包の重大な欠点は、メモリ内に新しい配列全体を構築してしまうことです。配列内包への入力自体が小さい配列であるときのオーバーヘッドは小さいのですが、入力が大きな配列や処理の多い (あるいは本当に無限の) ジェネレータであるときの配列の新規作成は問題になる場合があります。

ジェネレータはアイテムを必要なときに要求に応じて算出するため、一連のデータの計算処理を軽減します。ジェネレータ式は構文的に、配列内包とほとんど同じです。こちらは中括弧の代わりに丸括弧を使用して (また、for each...in の代わりに for...in を使用)、配列を構築する代わりに、すぐには実行されないジェネレータを作成します。これらは、ジェネレータ作成を簡略化した構文と考えることができます。

整数の大規模な数列に対して反復処理を行うイテレータ it を想定します。数列の値を 2 倍にする反復処理を行う、新たなイテレータを作成したいとします。配列内包では、2 倍の値を含むのに十分な配列をメモリ内に作成します:
{% highlight javascript %}
var doubles = [i * 2 for (i in it)];
{% endhighlight %}
一方ジェネレータ式は、必要なときに要求に応じて 2 倍の値を生成するイテレータを作成します:
{% highlight javascript %}
var it2 = (i * 2 for (i in it));
print(it2.next()); // it の最初の値を 2 倍にする
print(it2.next()); // it の 2 番目の値を 2 倍にする
{% endhighlight %}

ジェネレータ式が関数の引数として使用されるときは、関数の呼び出しで使用される丸括弧によりジェネレータ式の外側の丸括弧を省略できます:
{% highlight javascript %}
var result = doSomething(i * 2 for (i in it));
{% endhighlight %}