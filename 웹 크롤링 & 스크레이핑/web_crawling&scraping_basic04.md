# 웹 크롤링과 스크레이핑 04

> 저번까지는 정적인 크롤링을 주로 연습했고 이번부터는 동적인 크롤링을 연습해본다.





### Selenium을 이용한 동적 크롤링

```python
from selenium import webdriver #웹 컨트롤러
from selenium.webdriver.common.keys import Keys

browser=webdriver.Chrome()
browser.get("http://www.google.com")
l=browser.find_element_by_xpath("/html/body/div[1]/div[3]/form/div[1]/div[1]/div[1]/div/div[2]/input")
l.send_keys("뉴스") #검색어
l.send_keys(Keys.RETURN) #검색(엔터)
browser.implicitly_wait(10) #화면이 나올때까지 대기
ck=browser.find_element_by_xpath("//*[@id='rso']/div[2]/div/div/div[1]/div/a/h3")
ck.click() #클릭
browser.implicitly_wait(10)
browser.execute_script("window.scrollTo(0, 500);") #스크롤 이동
```

- 화면나올때까지 대기시간을 주어 최대한 자연스럽게 동작하도록 한다.





```python
from selenium import webdriver
from selenium.webdriver.common.keys import Keys
import pyperclip
id="aaa"
pw="123"

b=webdriver.Chrome()
b.implicitly_wait(10)
b.get("http://www.naver.com")
b.implicitly_wait(10)
lc=b.find_element_by_class_name("link_login")
lc.click()
b.implicitly_wait(10)

in_id=b.find_element_by_id('id')
b.implicitly_wait(10)
in_id.click()
pyperclip.copy(id) #클립보드에 복사
in_id.send_keys(Keys.CONTROL, 'v') #붙여넣기
b.implicitly_wait(10)

in_pw=b.find_element_by_id('pw')
b.implicitly_wait(10)
in_pw.click()
pyperclip.copy(pw) #클립보드에 복사
in_pw.send_keys(Keys.CONTROL, 'v') #붙여넣기
b.implicitly_wait(10)

b.find_element_by_id("log.login").click()
b.implicitly_wait(10)

news=b.find_element_by_class_name('nav')
news.click()
b.implicitly_wait(10)
```

- pyperclip은 클립보드라고 생각하면 편하다.
- 네이버에서 크롤링을 막기 위해 작동하는 알고리즘을 회피하기 위해 사용했다.





```python
from selenium import webdriver
from selenium.webdriver.common.keys import Keys
import pyperclip

id="aaa"
pw="123"

b=webdriver.Chrome()
b.implicitly_wait(10)
b.get("http://www.naver.com")
b.implicitly_wait(10)
lc=b.find_element_by_xpath("//*[@id='account']/a")
lc.click()
b.implicitly_wait(10)

in_id=b.find_element_by_xpath("//*[@id='id']")
b.implicitly_wait(10)
in_id.click()
pyperclip.copy(id) #클립보드에 복사
in_id.send_keys(Keys.CONTROL, 'v') #붙여넣기
b.implicitly_wait(10)

in_pw=b.find_element_by_xpath("//*[@id='pw']")
b.implicitly_wait(10)
in_pw.click()
pyperclip.copy(pw) #클립보드에 복사
in_pw.send_keys(Keys.CONTROL, 'v') #붙여넣기
b.implicitly_wait(10)

b.find_element_by_id("log.login").click() #로그인
b.implicitly_wait(10)

news=b.find_element_by_xpath("//*[@id='NM_FAVORITE']/div[1]/ul[2]/li[2]/a") #뉴스 들어가기
news.click()
b.implicitly_wait(10)

news=b.find_element_by_xpath("/html/body/section/header/div[2]/div/div/div[1]/div/div/ul/li[6]/a/span") #IT/과학 들어가기
news.click()
b.implicitly_wait(10)

news=b.find_element_by_xpath("//*[@id='snb']/ul/li[4]/a") #IT/일반 들어가기
news.click()
b.implicitly_wait(10)

data_l=[]
for i in range(1,11): #기사제목 10개 수집
    data_l.append(b.find_element_by_xpath(f"//*[@id='main_content']/div[2]/ul[1]/li[{i}]/dl/dt[2]/a").text)
    b.implicitly_wait(10)

for i in data_l: #수집한 기사제목 출력
    print(i)
```

- 뉴스 탭에서 뉴스 기사 제목 10개를 수집하는 실습 코드이다.
- 이미지가 없는 기사가 존재할 때는 오류가 생기기 때문에 xpath에 맞게 코드를 수정할 필요가 있다.