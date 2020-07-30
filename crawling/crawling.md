# 웹 크롤링(Crawling)

### 크롤링 절차 
1. HTML 소스코드 불러오기 (request)
2. HTML 소스코드 파싱 (BeautifulSoup)
3. 원하는 정보 추출

### BeautifulSoup
소스드를 파싱하는데에 사용하는 라이브러리
- .find : 태그명과 속성정보로 매칭되는 것 찾기(1개)
- .find_all : 태그명과 속성정보로 매칭되는 것 찾기(모두)
- .text : 태그의 텍스르를 출력
- .get : 태그의 속성정보 가져오기

### Selenium
웹 어플리케이션을 테스트하기 위해 고안된 프레임워크로 크롤링을 위해 웹을 동작시키는 데 유용하나, 속도면에서 단점이 있다.
- 로그인 등 웹을 동작시켜야만 정보가 나오는 경우 사용
- .get(URL) : 웹페이지로 이동
- .find_element_by ~ : 웹 요소를 찾음
- .click() : 찾은 웹 요소 클릭
- .send_keys(key) : 찾은 웹 요소에 정보를 보낸다.
- .page_source : html 소스코드 가져오기
