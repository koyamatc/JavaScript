---
title: JavaScript
layout: post
postTitle: Recursion
categories: basics
---

### Counting Up With Recursion
関数の終了する条件に出会うまで、その関数自身を繰り返し呼ぶことで、

*while* ループをまねることができます。

下の *countUp* はこのトリックを使って５まで数えています。

この方法（再帰呼出）でループを作る時のことを考えましょう。

繰返し中の各ステップでどんな動作が起きているのでしょう、

そして最後は？。関数は各ステップである動作（ *countUp* は数字を表示する。）
をし、自分自身を呼ぶことで、繰り返しの次のステップへと続けます。

これを __recursive case__ と呼びます。

繰り返しの最終ステップでは、関数はその動作を行い、そして終了します。

自分自身を呼ばないことで、繰り返しを終了します。

これを __base case__ と呼びます。

データがどのように次のステップへと受け渡されるのか？

再帰関数では、引数を使います。
{% highlight javascript %}
function countUp(current) {
    // Recursive case
    if (current < 5) {
        console.log(current);
        
        /* countUpを呼び、引数でもって情報を 
         受け渡すことで繰り返しを続けます。
         `current + 1` は `for` loopの`i++` 
        にあたります。*/
        countUp(current + 1);
    }
    
    // Base case
    if (current === 5){
        console.log(current);
        
        /* 最終ステップ。ここから"countUp" を
         呼ばないことで繰り返しをやめます。*/
    }
}

// 関数を呼ぶことで繰り返しを開始します。
countUp(1);
{% endhighlight %}

## Counting Down With Recursion
*for* ループでは、カウントダウンすることができました。

再帰関数でも同じことができます繰り返します。

*current* が１より大きいとき自分自身を呼び出し繰り返します。

受け渡す値は *current-1* 

繰り返しの終了は、 *current* が５になった時です。
{% highlight javascript %}
function countDown(current) {
    // Recursive case
    if (current > 1) {
        console.log(current);
        
       countDown(current - 1);
    }
    // Base case
    if (current === 1){
        console.log(current);
    }
}
countDown(5);
{% endhighlight %}

### Building 'substring' With Recursion (Part 1)
再帰呼び出しを使うことで、 *for* ループよりもっと簡単に *substring* を作ることができます。

今回作成する関数は３つの引数を取ります。

*all* :文字列全体

*start* :切り取る最初の文字位置

*end* :切り取る最後の文字位置

今回の作成プランは、 *all* から１文字づつ抜き取り、
 *start* を１づつ増加させていきます。
{% highlight javascript %}
var substring = function(all,start,end){

  console.log(all[start]);

// base case
  if (start>=end){
  }
};
{% endhighlight %}

### Building 'substring' With Recursion (Part 2)
*else* 文を追加し __recursive case__ を書く。
{% highlight javascript %}
var substring = function(all,start,end){
// base case
  if (start>=end){
  console.log(all[start]);
  } else {
// recursive case
    console.log(all[start]);
    substring(all,start+1,end);
  }
};
substring("lorem ipsum dolor",6,10);
{% endhighlight %}
      
### Building 'substring' With Recursion (Part 3)
ループを基にした *substring* では、
文字列に選択した１文字を付け加えていました。

ですがここではできません。

もし関数の外側に定義した文字列変数があれば、誰でも変更できます。

その変数が関数の中に定義されていたとしても、
 *substring* から再帰呼出しされた *substring* では
この変数にアクセスできません。

しかしこのことを文字列変数なしに行わなくてはなりません。

そこで, *return* 文を使い、この処理のために文字を送ります。
{% highlight javascript %}
var substring = function(all,start,end){

// base case
  if (start>=end){
    return all[start];
  } else {
    return all[start] + substring(all,start+1,end);
  }
};
console.log(substring("lorem ipsum dolor",6,10));
{% endhighlight %}
