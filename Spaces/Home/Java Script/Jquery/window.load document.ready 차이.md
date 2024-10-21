우선 $(document).ready()와 $(window).on("load") 함수가 어떤 역할을 하는지부터 짚고 넘어가시죠.

#### $(document).ready()

$(document).ready()의 경우 DOM(Document Object Model) 페이지가 JavaScript코드를 사용할 수 있을 때 실행이 됩니다. ([참조](https://learn.jquery.com/using-jquery-core/document-ready/))

*** 잠시 보는 DOM이란?**

DOM의 경우 Document Object Model로 한국어로 직역해보면 문서 객체 모델(...) 이며, 오늘 우리는 어려운 부분까지는 머리빠개지니까(??) 설명은 줄이고 그냥 쉽게 Front단에서 보여질 HTML이나 XML등의 문서를 객체로서 표현하여 가지고 있다고 생각을 하면 쉬울 것 같습니다.

이 DOM 덕분에 우리는 HTML을 JavaScript를 통해 요소나 속성등을 추가하고, 변경(삭제)를 할 수 있게됩니다.

**$(document).ready() 코드**

```
$(document).ready(function(){
	console.log("document ready!");
});
```

$(document).ready()함수의 매개변수로 실행시킬 function을 전달시킬 수 있습니다.

때문에 function을 위 코드와 같이 직접 선언을 하며 전달을 할 수 있고,

아래처럼 미리 다른곳에 만들어둔 function을 불러오는 방법도 사용할 수 있습니다.

```
$(document).ready(ready("document ready!"));

function ready(val) {
	console.log(val);
}
```

#### $(window).on("load")

$(window).on("load")의 경우에는 DOM을 포함한 모든 페이지가 준비가 되면 실행이 됩니다. (참조)

때문에 $(document).ready()보다 늦게 실행이 되는 모습을 보여줍니다.

**$(window).on("load") 코드**

```
$(window).on("load", function() {
	console.log("window load!");
});
```

$(window).on("load")함수는 위 $(document).ready()와는 조금 다른 모습을 띄고 있습니다.

예~~~~전 버전 jQuery에서는 $(window).load()와 같이 위 $(document).ready()와 거의 비슷한 형태를 띄고 있었으나, jQuery 버전(3.0 이상부터)이 올라가면서 .load()함수는 제거되었다고 합니다. 

$(window).on("load") 또한 매개변수로 실행시킬 function을 넘겨줄 수 있습니다.

위와같이 선언과 동시에 전달을 하는 방법이 있고, 아래처럼 function을 불러오는 방법도 가능합니다.

```
$(window).on("load", function() {
	ready("window load!")
});

function ready(val) {
	console.log(val);
}
```

#### $(document).ready()와 $(window).on("load")의 실행 순서

위 코드를 직접 console에 찍어보면 아래와 같은 결과를 확인하실 수 있습니다.

![[Pasted image 20241021104242.png]]

![](https://blog.kakaocdn.net/dn/CoTkd/btqGfs9ryxj/vTU0KehJef14o3n8TaEnr0/img.png)

console에서 찍어본다면.

위 설명에 적어놓은 것 처럼 DOM객체가 JavaScript를 실행할 수 있는 상태가 된다면 $(document).ready를, 모든 페이지에 대한 준비가 완료된다면 $(window).on("load")함수가 실행이 된다는걸 확인할 수 있습니다.

**대략 이런느낌**


![[Pasted image 20241021104250.png]]

음.. 내일부터는 또 한 주의 시작이네요.

코로나는 끝을 보이지 않고, 비도 많이와서 다들 지치실 것 같은데, 힘내시기 바랍니다.

모든분들 언제나 응원하고 있습니다!




---


출처 - https://karzin.tistory.com/221