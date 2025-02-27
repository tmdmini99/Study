# 웹 크롤러란?
크롤링(crawling) 혹은 스크레이핑(scraping)은 웹 페이지)를 그대로 가져와서 거기서 데이터를 추출해 내는 행위다. 크롤링하는 소프트웨어는 크롤러(crawler)라고 부른다.


**웹 크롤러(Web Crawler)** 는 **방대한 웹 페이지를 두루두루 방문하여, 각종 정보를 자동적으로 수집하는 일을 하는 프로그램으로서 검색 엔진의 근간**이 됩니다. 크롤러(crawler)란 기어가는 사람 혹은 포복동물 이라는 의미로, 조직적, 자동적인 방법으로 각종 웹 페이지들을 돌아다니며 웹 문서의 URL, 링크정보, 문서내용 등 다량의 정보들을 수집해 오는 기능으로 인해 이런 이름이 붙었답니다. 웹 크롤러에 대한 다른 용어로는 앤트(Ants), 자동인덱서(automatic indexers), 봇(bots), 웜(worms), 웹 스파이더 (web spider), 웹 로봇(web robot) 등이 있답니다.

![[Crawler1.png]]

웹 크롤러가 하는 작업을 웹 크롤링(web crawling) 혹은 스파이더링(spidering)이라고 부르기도 하는데요, 검색 
엔진과 같은 여러 사이트에서는 데이터의 최신 상태 유지를 위해 항상 웹 크롤링을 합니다. 웹 크롤러는 대체로 방문한 사이트의 모든페이지의 복사본을 생성하는 데 사용됩니다.

> 크롤링
> 	웹상의 정보들을 탐색하고 수집하는 작업을 의미
> 	URL을 탐색해 반복적으로 링크를 찾고 가져오는 과정

또한 크롤러는 링크체크나 HTML 코드 검증과 같은 웹 사이트의 자동 유지 관리 작업을 위해 사용되기도 하며, 자동 이메일 수집과 같은 웹 페이지의 특정 형태의 정보를 수집하는데도 사용됩니다. 웹 크롤러는 봇이나 소프트웨어 에이전트의 한 형태로 대개 시드(seed)라고 불리는 URL리스트에서부터 시작하며 페이지의 모든 하이퍼링크를 인식, URL 리스트를 갱신하여 확인합니다.

- Web상의 정보들을 탐색하고 수집하는 작업을 의미.
    
- 규칙에 따라 자동으로 웹 문서를 탐색하는 컴퓨터 프로그램
    
- 검색 포털(네이버, 구글)에 키워드를 입력하면 해당 포털의 URL을 지닌 페이지 뿐만 아니라 외부 사이트도 본문의 요약본을 만드는 것.



웹 크롤러는 인터넷 공간 여기저기를 돌아다니며 정보를 수집한다는 특징 때문에 사람들에게 피해를 주는 존재라고 생각할 수 있습니다. 실제로 **웹 크롤러는 설계(프로그래밍)를 잘못하면 네트워크에 트래픽을 증가시키고 서버에 과부하를 줄 수 있으며, 블로그나 카페 등에 게시한 개인정보까지 수집**해가기 때문입니다. 게시한 글을 지우더라도 웹 크롤러가 수집하여 검색엔진 데이터베이스에 저장한 정보는 지워지지 않으며, 나중에 다른 사용자가 검색할 수도 있습니다. 구글이나 네이버 등 검색 시 볼 수 있는 저장된 페이지가 바로 그것이죠.
그렇다면 이 웹크롤러는 트래픽 증가의 주범이자 사생활을 침해하는 나쁜 프로그램일까요?  모든 것이 장단점이 있듯 **웹 크롤러는 검색엔진 외에도 링크 체크, HTML코드 검증, 자동 이메일 수집 등 다양한 형태로 사용됩니다. 이를 통해 사람이 손으로 하기 귀찮은 수많은 작업을 자동으로 수행**해주고 있답니다. 


![[Crawler2.png]]

이미 웹 크롤러가 수집해간 정보를 DB에서 지우려면 해당 서비스의 고객센터에 연락해 삭제 요청하는 방법밖에 없습니다. 이런 이유로 가급적 중요한 정보가 담긴 게시물은 비공개로 설정 혹은 회원만 볼 수 있도록 설정하는 것이 좋습니다. 웹 크롤러는 해당 웹 페이지에 원천적으로 접근할 수 없어 열람이 불가능하기 때문입니다. 기업이나 개인 홈페이지를 운영하는 사람이라면<robots.txt>라는 텍스트 파일을 사용해 웹 크롤러를 배제하는 방법이 있습니다. 해당 텍스트 파일 안에 여러 가지 코드를 넣어 특정 웹 크롤러가 접근하지 못하도록 차단할 수 있답니다.


# 스크래핑

>우리가 정한 특정 웹 페이지에서 데이터를 추출하는 것
>- 자동으로 수집된 특정 정보가 필요한 분야에서 다양하게 활용(금융, 주식, 전자상거래 시장 등)
>- 특정 사이트나 페이지에 대한 데이터를 찾으므로, 정확한 정보를 요구할 때 유용( 주식 정보, 뉴스 등)

웹 스크래핑은 특정 웹 사이트나 페이지에서 필요한 데이터를 자동으로 추출해 내는 것을 의미합니다. 웹 스크래핑은 다음과 같이 작동합니다. 원하는 정보를 추출하기 위해 ‘스크래퍼 봇’이 특정 웹 사이트에 콘텐츠를 다운로드하기 위한 HTTP GET 요청을 보냅니다. 사이트가 이에 응답하면 스크래퍼는 HTML 문서를 분석하여 특정 패턴을 지닌 데이터를 뽑아냅니다. 그리고 추출된 데이터를 원하는 대로 사용할 수 있도록 데이터베이스에 저장합니다.

웹 스크래핑은 자동으로 수집된 특정 정보가 필요한 분야에서 다양하게 활용되고 있습니다. 금융 및 주식 시장의 경우, 스크래핑 기술을 활용하여 뉴스 정보를 모으기도 하고, 애널리스트들이 투자 자문을 위해 활용할 수 있는 기업 재무제표 정보를 자동으로 수집하기도 합니다. 전자상거래 시장의 경우 경쟁력 확보를 위해 경쟁사 상품의 정보를 수집하고 가격 변동 이슈를 빠르게 파악하기 위해 스크래핑 기술을 활용하기도 하죠.




#  웹 크롤링과 웹 스크래핑의 장점


>  분석과 실시간 정보 제공에 유용한 “웹 크롤링”

웹 크롤링은 웹상을 돌아다니며 방대한 양의 정보를 수집하기 때문에, 특정 키워드에 대한 심층 분석이 필요할 때 유용합니다. 또한 크롤러는 실시간 정보 수집을 위해 계속해서 작동하므로 자주 변화하는 데이터를 파악하기가 좋습니다.

> 정확한 정보를 요구할 때 쓰이는 “웹 스크래핑”

웹 스크래핑은 특정 사이트나 페이지에 대한 정보를 찾는데 집중하므로 데이터 포인트를 정확히 잡고 확실한 정보만을 수집할 수 있다는 점에서 유용합니다. 장기적으로 서비스 대역폭이나 비용을 절약할 수 있다는 장점이 있습니다.

# 웹 크롤링과 웹 스크래핑의 차이점

크롤링과 스크래핑은 ‘원하는 데이터를 모을 수 있다’는 점이 비슷하여 의미가 자주 혼용되곤 합니다. 또한 기술적으로 함께 사용되는 경우가 많아 더욱 헷갈립니다. 하지만 웹 크롤링은 웹 페이지의 링크를 타고 계속해서 탐색을 이어나가지만, 웹 스크래핑은 데이터 추출을 원하는 대상이 명확하여 특정 웹 사이트만을 추적한다는 차이점이 있습니다.

또한 웹 크롤링은 페이지를 모아 색인화(분류)하고 검색 결과에 내가 찾는 키워드와 연관된 링크들만 모아 볼 수 있도록 작동합니다. 하지만 웹 스크래핑은 상품의 가격, 주식 정보, 뉴스 등 원하는 데이터가 명확하며, 흩어져있는 해당 데이터를 자동으로 추출하여 전달합니다. 

**웹 크롤링**은 특정 웹 페이지를 목표로 하지 않습니다. 일단 탐색부터 하고, 정보를 가져오죠. **'선탐색 후추출'** 입니다. 반면 **웹 스크래핑**을 할 때는 목표로 하는 특정 웹페이지가 있습니다. 우리가 원하는 정보를 어디서 가져올지 타겟이 분명하고, 그 타겟에서 정보를 가져오죠. 그래서 **'선결정 후추출'** 입니다.


![[Crawler3.png]]


# 크롤러 시스템 구현

 
pom.xml 추가
```xml
<!-- https://mvnrepository.com/artifact/org.jsoup/jsoup -->
<dependency>
    <groupId>org.jsoup</groupId>
    <artifactId>jsoup</artifactId>
    <version>1.16.1</version>
</dependency>

```












---
참조 -  https://kkyunstory.tistory.com/2


https://velog.io/@kyy00n/%EB%8C%80%EA%B7%9C%EB%AA%A8-%EC%8B%9C%EC%8A%A4%ED%85%9C-%EC%84%A4%EA%B3%84-%EA%B8%B0%EC%B4%88-9%EC%9E%A5.-%EC%9B%B9-%ED%81%AC%EB%A1%A4%EB%9F%AC-%EC%84%A4%EA%B3%84


https://offbyone.tistory.com/116


https://brunch.co.kr/@bamchi/12

https://velog.io/@bbkyoo/%EC%9B%B9-%ED%81%AC%EB%A1%A4%EB%A7%81%EA%B3%BC-%EC%9B%B9-%EC%8A%A4%ED%81%AC%EB%9E%98%ED%95%91-%EA%B0%9C%EB%85%90

https://dev-gorany.tistory.com/12
