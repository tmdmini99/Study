**모듈 만들기**

![[Pasted image 20240910144529.png]]

프로젝트에 app.js 와 movie.js를 만든다.

여기서 movie.js 는 모듈이 되고, app.js에서는 movie.js 모듈을 불러와 사용할 것이다.

먼저 movie.js를 작성해보자

**movie.js**

```
// movie 라는 모듈을 만든다.
function printHarryPotter(){
    console.log("Harry Potter!");
}
 
function printDawnOfDead(){
    console.log("Dawn Of Dead!");
}
 
module.exports.HarryPotter = printHarryPotter;
module.exports.DawnOfDead = printDawnOfDead;​
```

영화 이름을 찍어주는 2개의 함수를 만들어보자.

printHarryPotter()와 printDawnOfDead() 를 만들었다.

이제 이 함수를 app.js에서 사용 하려면, module.exports를 이용해서 무엇을 exports 할건지 작성해줘야 한다.

형식은 아래와 같다.

module.exports.자신이 원하는 이름 = 함수;

쉽게 변수 = 값 이라고 생각하면 된다.

사용할때는 내가 설정한 변수명을 이용해서 사용하면 된다.

이렇게 만든 모듈을 app.js에서 사용을 해보자.

**app.js**

```
var movie = require('./movie');
movie.HarryPotter(); // movie.js에서 exports 한 이름으로 호출하면 사용할 수 있다.
movie.DawnOfDead();
```

require()함수를 이용해서 movie 모듈을 가져온다. 

이때 확장자 명은 붙이지 않는다.

같은 디렉토리 안에서 movie라는 이름의 모듈을 찾아, 가져온다. 그리고

var movie 변수안에 할당 한다.

movie.설정한 변수명

을 이용해서 함수를 호출할 수 있다.

위의 경우는 movie.js 에서 exports할때 이름을 HarryPotter, DawnOfDead로 했기 때문에,

그 이름을 가지고 호출 하면 된다.

**실행 결과**

```
Harry Potter!
Dawn Of Dead!
```





---

출처 - https://hongku.tistory.com/92