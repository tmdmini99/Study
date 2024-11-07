



# Cloudflare is free of CAPTCHAs; Turnstile is free for everyone

> Now that we’ve eliminated CAPTCHAs at Cloudflare, we want to make it easy for anyone to do the same, even if they don’t use other Cloudflare services

클라우드플레어에서 캡챠(Captchas) 의 대안으로 "턴스타일(Turnstile)" 을 무료로 배포하기 시작했다. (10월 공식 뉴스) 아마 가장 쉽고 흔하게 사용되는 캡차, "[리캡챠](https://namu.wiki/w/reCAPTCHA)" 는 익숙할 것 이다.

1년 전부터 '사용자 친화적이고 개인 정보를 보호하는 캡차(CAPTCHA) 대체 수단'으로 시범 출시된 Turnstile은 이전에 문제를 성공적으로 통과한 방문자의 공통된 특징을 감지하는 머신 러닝 모델을 활용한다고 한다.

## 1. 기존 캡챠 문제

사실 클라우드플레어는 과거부터 캡챠를 없애고 싶어했다. **_사용자들의 15% 시간이 캡챠에 낭비_** 된다고 하며, **_"사람들은 캡차 질문에 답하는 데 날마다 약 500년을 소비하여 시간을 낭비하고 좌절감을 느끼고 있다."_** 라고 한다

더욱이 요즘 봇들은 사람보다 캡챠를 더 잘푸는 경우도 많다. "오디오 CAPTCHA" 는 사람이 풀기에는 시각적 CAPTCHA보다 훨씬 더 나쁘며, 오디오 챌린지의 경우 31.2% 정도의 성공율을 보인다고 한다. 그리고 최근 연구에 따르면 봇은 85% 이상의 시도에서 오디오 CAPTCHA를 정확하게 푼다고 한다. 이미지 역시 점점 봇이 더 잘풀고 있다. 



## 2. 더 나은 점

1. 웹 콘텐츠 접근성 가이드라인(Web Content Accessibility Guidelined, WCAG 2.1)
2. ePrivacy, GDPR 및 CCPA 준수 요건 (개인 정보 보호 관련)
3. 사람에게 더 친화적이고 봇에게는 더욱 더 어렵게
4. 적용은 더 쉽고 빠르게 그리고 무료

사람에게 퀴즈를 내지 않는다! 그런데 어떻게 봇인이 아닌지 감지를 하는 것일까? 단순 체크 박스 클릭하나로? **_"the actual act of checking a box isn’t important, it’s the background data we’re analyzing while the box is checked that matters"_** 라고 한다.

행위가 중요한게 아니라 박스가 선택될 때 수집되어 분석되는 데이터가 핵심이라고 한다. 네이티브 브라우저의 API를 호출하고, 가볍고 빠른 테스트를 진행한다고 한다. (ex: proof-of-work tests, proof-of-space tests) 그리고 1년간 베타 중 "진짜 브라우저" 에 대한 데이터를 학습 (머신 러닝)을 진행한 것 같다. 아마 이 과정에서 E2E testing 도구로 사용되는 셀레니움 등을 필터링 한다고 여겨진다.

## 3. 찍어먹어 보기


![[Pasted image 20241107132119.png]]

[일단 개발자 문자가 잘 되어 있다.](https://developers.cloudflare.com/turnstile/) 그리고 리액트 라이브러리도 존재한다. [깃허브 데모 레포](https://github.com/cloudflare/turnstile-demo-workers) 에서 빠르게 적용된 코드 역시 확인 가능하다 .

그리고 Enterprise에는 SaaS 플랫폼 지원과 Cloudflare 로고를 없앨 수 있다. "셀프 서비스 고객은 2024년 초에 고급 기능에 대한 유료 옵션이 제공될 것으로" 계획하고 있다고 한다. **_사용자는 베타 기간과 마찬가지로 100만 건의 사이트 확인 요청 한도 미만으로_** Turnstile의 고급 기능에 계속 액세스할 수 있다고 한다. 고급 기능은 클라우드플레어의 계정을 생성하고, 추가 설정을 할 필요가 있다.

### 1) getting started

```bash
git clone https://github.com/cloudflare/turnstile-demo-workers.git
cd turnstile-demo-workers
npm install
npm run dev
```


![[Pasted image 20241107132128.png]]



![[Pasted image 20241107132131.png]]



그리고 사실 이게 끝이다! 데모 페이지를 구경하면 된다! 사실 체크도 의심되는 사용자에게만 해달라고 요청하고, 자명한 유저에 대상으로는 자동 체크가 된다. 이 간극이 거의 1초라고 한다!

### 2) detail

```html
<script src="https://challenges.cloudflare.com/turnstile/v0/api.js" async defer></script>
```



![[Pasted image 20241107132140.png]]



간단하게 살펴보면 cloudflare의 `api.js` 를 import 해서 가져오면 해당 파일에서 `IIFE, Immediately Invoked Function Expression` 처리된 js code로 아래 2가지 api를 호출하게 된다. ~~자 이제 저 2개의 파일, html과 script응답만 분석하면 turnstile을 파훼할 수 있는 방법을 알게 될지도 모른다!~~

일단 적용하기 엄청 편하다. 특히 form 이 있는 곳 어디든 쉽고 빠르고 편하게 적용이 가능하다는 것이다.

### 3) 고오급 기능 맛보기


![[Pasted image 20241107132151.png]]



![[Pasted image 20241107132155.png]]


아래 docs를 보고 복사 - 붙여넣기

![[Pasted image 20241107132208.png]]


#### 참고로 더 쉽게하는 방법이 있다.

일단 Turnstile 등록을 했으면, 깃허브 레포에서 가져온 코드를 바꾸면 된다. 그러면 진짜 배포버전을 맛볼 수 있다. 아래 부분의 값을 위에서 만든 값으로 바꿔주자


![[Pasted image 20241107132221.png]]


![[Pasted image 20241107132223.png]]


![[Pasted image 20241107132226.png]]


**_"`npm run deploy`"_** 를 사용하면 된다. 그러면 [cloud flare server less platform인 workers](https://ryanking13.github.io/2020/07/26/introducing-cf-workers-2.html) 라는 앱으로 등록이 된다. (이 과정에 cloud flare 인증이 조금 필요하다.) 그러면 끝!


---

출처 - https://velog.io/@qlgks1/%EC%9D%B4-%EC%BA%A1%EC%B1%A0%EB%8A%94-%EB%AC%B4%EB%A3%8C%EB%A1%9C-%ED%95%B4%EC%A4%8D%EB%8B%88%EB%8B%A4