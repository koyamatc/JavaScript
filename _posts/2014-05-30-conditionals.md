---
title: JavaScript
layout: post
postTitle: Conditionals
categories: basics
---

## 条件式

### if else 文

#### if 文

{% highlight javascript %}
var yourName = "";
var result = "";
if (yourName.length===0) {
  result = "What is your name?";
};
{% endhighlight %}

####条件に合うもの、合わないもの

{% highlight javascript %}
var yourName = "NIck";
var result =  "";

if (yourName.length > 0) {
   result = "Hi "+ yourName;
} else {
   result = "What is your name?";
};
{% endhighlight %}

####複数の条件をテスト

{% highlight javascript %}
var yourName = "take";
var gender = "female";
var result;

if (gender==="male") {
  result = "His name is "+yourName;
} else if (gender==="female") {
  result = "Her name is "+yourName;
} else {
  result = "Hi "+yourName;
};
{% endhighlight %}

####1つの条件を満たす
{% highlight javascript %}
var yourName = "Take";
var gender = "female";

// don't forget to use the || operator 
if (gender === "male" || gender === "female"   ) {
  result = true;
} else {
  result = false;
};
{% endhighlight %}

####両方の条件を満たす
{% highlight javascript %}
var yourName = "Take";
var gender = "male";
var result;

if (yourName.length>0 && gender.length > 0) {
  result = "Thanks";
} else {
  result = "Please make sure both yourName and gender are filled in.";
};
{% endhighlight %}
 
####条件文の入れ子
{% highlight javascript %}
var yourName = "Take";
var gender = "male";
var result;

if (yourName.length > 0 && gender.length > 0) {
  if (gender ==="male" || gender==="female") {
    result = "Thanks";
  } else {
    result = "Please enter male or female for gender.";
  }
} else {
  result = "Please tell us both your name and gender.";
};
{% endhighlight %}
 
-----
###switch 文
{% highlight javascript %}
var jacketColor = "green";
var result;

switch (jacketColor) {
    
  case "black":
    result = "Pay $300";
    break;
    
  case "brown":
    result = "Pay $200";  
    break;
    
  case "green":
    result = "Pay $5";
    break;
    
  default:
    result = "This color does not match my eyes!";
};
{% endhighlight %}

----

###Ternary operators

_result = x > y ? "good job" : 20;_

右辺の結果を左辺に代入する。

_x > y ?_

_"good job" : 20;_

条件式が
  
  真の時 _:_ の左側の値が _result_ へ代入される
  
  負の時 _:_ の右側の値が _result_ へ代入される

{% highlight javascript %}
var x = 2;
var y = 3;

if (x > y) {
  result = "good job";
}
else {
  result = 20;
}

//上記を ternary operator で書くと
result = x > y ? "good job" : 20;
{% endhighlight %}