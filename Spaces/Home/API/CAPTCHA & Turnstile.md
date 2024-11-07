



# Cloudflare is free of CAPTCHAs; Turnstile is free for everyone

> Now that we’ve eliminated CAPTCHAs at Cloudflare, we want to make it easy for anyone to do the same, even if they don’t use other Cloudflare services

클라우드플레어에서 캡챠(Captchas) 의 대안으로 "턴스타일(Turnstile)" 을 무료로 배포하기 시작했다. (10월 공식 뉴스) 아마 가장 쉽고 흔하게 사용되는 캡차, "[리캡챠](https://namu.wiki/w/reCAPTCHA)" 는 익숙할 것 이다.

1년 전부터 '사용자 친화적이고 개인 정보를 보호하는 캡차(CAPTCHA) 대체 수단'으로 시범 출시된 Turnstile은 이전에 문제를 성공적으로 통과한 방문자의 공통된 특징을 감지하는 머신 러닝 모델을 활용한다고 한다.

## 1. 기존 캡챠 문제

사실 클라우드플레어는 과거부터 캡챠를 없애고 싶어했다. **_사용자들의 15% 시간이 캡챠에 낭비_** 된다고 하며, **_"사람들은 캡차 질문에 답하는 데 날마다 약 500년을 소비하여 시간을 낭비하고 좌절감을 느끼고 있다."_** 라고 한다

더욱이 요즘 봇들은 사람보다 캡챠를 더 잘푸는 경우도 많다. "오디오 CAPTCHA" 는 사람이 풀기에는 시각적 CAPTCHA보다 훨씬 더 나쁘며, 오디오 챌린지의 경우 31.2% 정도의 성공율을 보인다고 한다. 그리고 최근 연구에 따르면 봇은 85% 이상의 시도에서 오디오 CAPTCHA를 정확하게 푼다고 한다. 이미지 역시 점점 봇이 더 잘풀고 있다. 


![[Pasted image 20241107110038.png]]


---

출처 - https://velog.io/@qlgks1/%EC%9D%B4-%EC%BA%A1%EC%B1%A0%EB%8A%94-%EB%AC%B4%EB%A3%8C%EB%A1%9C-%ED%95%B4%EC%A4%8D%EB%8B%88%EB%8B%A4