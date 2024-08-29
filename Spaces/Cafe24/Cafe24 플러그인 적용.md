
![[Pasted image 20240829151508.png]]

카페 24 페이지에서 디자인(PC/모바일) 클릭

![[Pasted image 20240829151552.png]]

디자인 보관함 -> 디자인 편집 클릭

![[Pasted image 20240829151713.png]]

접속 후  HTML 수정 클릭 


![[Pasted image 20240829151859.png]]


전체화면 보기 -> 레이아웃 -> 기본레이아웃 접속후 요청 script 코드 를 layout에 전부 추가 
(공급사 레이아웃, 공통레이아웃,메인 레이아웃, 인트로 레이아웃, 팝업레이아웃 등)


\</body> 바로 위쪽에 추가
ex code

배너매니저 스크립트 코드를 \</body>  바로 위에 추가

```
    <!-- 배너매니저 스크립트 코드 -->
<style>[df-banner-code][hidden] { display:none !important }</style>
<script>
if (!document.getElementById('dfbm-script-44073')){
  var dfbm_src = 'https://ecimg.cafe24img.com/pg1281b19860949066/tmdals23222/web/upload/appfiles/ZaReJam3QiELznoZeGGkMG/4ef3e5c5a151a1c8208cef9add99abd4.js';
  document.write('<script id="dfbm-script-44073" src="' + dfbm_src + '?v=' + Date.now() + '"><\/script>');
}
</script>
</body>
```


![[Pasted image 20240829152109.png]]

그 후 원하는 위치에  원하는 곳에 플러그인 코드 추가

ex

```
<div df-banner-code="test" hidden>
        <h2>test코드</h2>
        <div df-banner-clone>
            <a href="{#href}">{#item}</a>
    	</div>
    </div>    
```

![[Pasted image 20240829152227.png]]


메인 화면에 적당한 위치에 플러그인에서 적용하라는 코드를 직접 추가


![[Pasted image 20240829152357.png]]

테스트 배너 생성 후  index.html에 추가


ex 배너 매니저 html 추가 코드

```

**작성예)**

**<div df-banner-code="test" hidden>**

    **<h2>안녕하세요?</h2>**

    **<a href="{#href}" target="{#target}" df-banner-clone>{#item}</a>**

**</div>**

  

**결과물)** 

위와 같은 소스를 삽입하면 실제 쇼핑몰에 아래와 같은 마크업으로 표시됩니다.

  

**<div df-banner-code="test" class="df-bannermanager df-banner-manager-test">**

    **<h2>안녕하세요?</h2>**

    **<a href="배너매니저로 등록한 링크 URL"><배너매니저로 등록한 이미지></a>**

**</div>**
  

**※작성 규칙과 변수※**

  

**1.그룹 애트리뷰트 추가**

df-banner-code="그룹코드" 와 hidden 애트리뷰트를 추가하여 태그를 작성합니다.

※그룹코드에는 연결할 그룹의 코드를 입력하세요.

  

**작성예)** 

**<****div df-banner-code="그룹코드" hidden>**

**</div>**

  

**2.반복 애트리뷰트 추가**

자식 요소에 df-banner-clone 애트리뷰트를 추가하여 태그를 작성 합니다.

※반복 애트리뷰트는 한개의 그룹당 한개만 사용 가능합니다.

※배너가 한개이상 등록될 경우 해당 애트리뷰트가 입력된 태그가 반복되어 출력됩니다.

  

**작성예)** 

**<div df-banner-code="그룹코드" hidden>**

    **<div df-banner-clone>**

    **</div>**

**</div>**

  

  

**3.변수 추가**

이제 df-banner-clone 애트리뷰트의 자식요소에 변수를 입력하여 태그를 작성합니다.

  

**작성예)**

**<div df-banner-code="그룹코드" hidden>**

    **<div df-banner-clone>**

        **<a href="{#href}">{#item}</a>**

    **</div>**

**</div>**

  


**[필수] 애트리뷰트 설명**  

**df-banner-code="그룹코드"** 배너매니저 앱에서 생성한 그룹과 연결하기위한 애트리뷰트입니다.

**hidden** 배너의 출력전 HTML에 입력된 변수명을 숨기기위한 애트리뷰트입니다. 앱이 실행 됨과 동시에 해당 애트리뷰트는 제거됩니다.

**df-banner-clone** 배너가 한개이상 등록될 경우 해당 애트리뷰트가 입력된 태그가 반복되어 출력됩니다.

  


**[선택] 애트리뷰트 설명**

**df-banner-wrap** 해당 애트리뷰트를 사용한 태그 내에 표시상태를 on 으로 설정한 배너가 없는 경우 태그 전체를 제거합니다.

*배너매니저는 기본적으로 그룹off 설정 시 그룹이 제거됩니다. 따라서 해당 기능은 필수 기능이 아닙니다. 

 본 기능은 여러개의 배너 그룹을 한 데에 묶어서 표현하거나, 배너 외의 요소를 사용한 경우 부모에 삽입하여 전체를 ON/OFF 하는 용도로 제작되었습니다.

  

**[선택] 제공 변수 설명**

변수는 반복 애트리뷰트인 df-banner-clone 의 자식요소에서만 작동됩니다.

제공되는 변수는 선택적으로 입력하여 활용이 가능하며, 다양한 태그 규칙으로 작성하실 수 있습니다.

**{#href}** 링크URL에 입력한 값이 출력됩니다.

**{#target}** 설정한 링크 타겟 값이 출력됩니다.

**{#title}** 배너이름에 입력한 값이 출력됩니다.

**{#index}** 등록한 배너의 번호가 0, 1, 2, 3 순으로 출력됩니다.

**{#item}** 등록한 이미지 또는 아이프레임이 출력됩니다. 배너이름이 alt 값으로 포함되어 출력됩니다.

**{#type}** 첨부파일 형식에 따른 결과값을 출력합니다. ( 이미지: df-bannermanager-type_image ) ( 아이프레임: df-bannermanager-type_iframe )

**{#html}** HTML 코드에 입력한 태그가 출력됩니다. **_new_**


```



