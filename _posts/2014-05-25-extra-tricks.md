---
title: JavaScript
layout: post
postTitle: Extra Tricks
categories: basics
---

## Looping Through Objects
オブジェクトの中の情報すべてを表示するには、どうすればよいでしょう？

配列では、繰り返し処理で指標を使いすべての要素名を取得できました。

オブジェクトの場合も似ています。指標ではなくプロパティ名を持ち、
つまり"keys"を持っています。

特別な *for* ループを使って、オブジェクト内の
すべてのキーを順番に処理します。
{% highlight javascript %}
var person = {
    name: "Morgan Jones",
    telephone: "(650) 777 - 7777",
    email: "morgan.jones@example.com"
};

for (var propertyName in person) {
    console.log(propertyName + ": " + person[propertyName]);
}
{% endhighlight %}

### Nesting 'for' Loops
ループの入れ子に制限はありません。

*for* ループの中に *while* ループ、

*for* ループの中に *for* ループ、などなど。
{% highlight javascript %}
var i = 0;
var j = 0;
for (i=1;i<=3;i++){
  for (j=1;j<=5;j++){
    console.log(j);
  }
}
{% endhighlight %}

## Printing Out Tables (Part 1)
ここでの小技は、配列の中に配列を置くというものです。

下のコードでは、この技を使ってスプレッドシートのテーブルのようなデータ形式を表現しています。

配列　*table* は、内部に複数の配列を持っています。

中の配列それぞれは行を表します。各配列の要素はセル表します。
入れ子のループを使って、 *table* の各要素にアクセスします。

変数 *rows* を定義して、 *table* の大きさをセットします。

変数 *r* は、 *for* ループの繰り返しに使用します。

まずは変数 *r* を表示してみます。
{% highlight javascript %}
var table = [
    ["Person",  "Age",  "City"],
    ["Sue",     22,     "San Francisco"],
    ["Joe",     45,     "Halifax"]
];

var rows = table.length;
var r;
for(r=0;r < rows;r++){
  console.log(r);
}
{% endhighlight %}

## Printing Out Tables (Part 2)
{% highlight javascript %}
var table = [
    ["Person",  "Age",  "City"],
    ["Sue",     22,     "San Francisco"],
    ["Joe",     45,     "Halifax"]
];

var rows = table.length;
var r;
for(r=0;r < rows;r++){
  var c;
  var cells = table[r].length;
  var rowText = "";
  for(c=0;c < cells;c++){
    rowText += table[r][c];
    if(c < cells){
      rowText += "  ";
    }
  }

  console.log(rowText);
}
{% endhighlight %}
      
      
## Breaking Out
ループをある条件で、即座に終了したいときに
 *break* 文が使えます。
*for,while* 両方で使えますが、再帰呼出では使えません。
{% highlight javascript %}
while (true) {
    console.log("I only print once!");
    break;
}
{% endhighlight %}