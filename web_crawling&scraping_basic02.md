# 웹 크롤링과 스크레이핑 02

> 01에서 배웠던 것보다 쉬운 방법으로 웹페이지를 추출해보고 정적인 환경에서 데이터를 직접 추출해본다.



### 웹 페이지 간단하게 추출하기

> requests를 사용하면 urllib를 사용하는 것 보다 훨씬 간단하게 웝페이지를 추출할 수 있다.



```python
import requests

r=requests.get("http://www.hanbit.co.kr/store/books/full_book_list.html")
r.raise_for_status() #웹페이지에 접근하지 못하면 하위 코드 실행하지 않게 함

with open("data.html", "w", encoding='utf-8') as f:
    f.write(r.text)
```



---

### 정규식 표현

> 텍스트 데이터를 추출할 때 알아둬야 할 정규식 표현이다. 상황에 맞게 다양한 정규식 표현을 사용하여
>
> 텍스트 데이터를 추출한다.



```python
#정규식 표현
#.=문자
#^=시작
#$=끝
#정규식: 원하는 형태에 따른 문자열 선택할 수 있게 해줌
#match("문자열"): 처음부터 일치하는지 검사
#search("문자열"): 일치하는 문자가 있는지 검사
#findall("문자열"): 일치하는 모든 것의 리스트 출력

import re

def print_t(str):
    if str:
        print("일치문자", str.group())
        print("입력문자", str.string)
        print("일치문자 시작", str.start())
        print("일치문자 끝", str.end())
        print("일치문자 시작, 끝", str.span())
    else:
        print("일치 없음")

l=['abcd','adcd','accd','abdc', 'casdfd', 'cabcdd', 'c1234d', 'cddddd']
ck=re.compile('^c....d$')
for i in l:
    str=ck.match(i)
    print_t(str)
    str=ck.search(i)
    print_t(str)
    str=ck.findall(i)
    print(str)
```



---

### Beautiful Soup로 스크레이핑하기

> Beautiful Soup를 사용하여 간단하게 스크레이핑할 수 있다.



```python
import requests
from bs4 import BeautifulSoup

url="http://www.hanbit.co.kr/store/books/full_book_list.html"
r=requests.get(url) #html 가져오기

soup=BeautifulSoup(r.text, "html.parser") #가져온 html 정리

#가져온 html에서 title이라는 태그를 가지고 있는 값들을 텍스트로 받아온다.
print(soup.title.get_text()) 

#가져온 html에서 a태그의 링크를 받아온다.
print(soup.a['href'])

#가져온 html에서 a태그의 링크가 /store/books/look.php?p_code=B9483006177인 값을 찾아 텍스트로 받아온다.
#실행하면 책 이름이 출력된다.
print(soup.find('a', attrs={"href":"/store/books/look.php?p_code=B9483006177"}).get_text())

#설정한 태그를 포함하는 모든 내용을 찾는다
print(soup.find_all('div'))
```



1. next_sibling

   - next_sibling으로 다음 태그에 있는 내용에 접근할 수 있다.

   ```python
   import requests
   from bs4 import BeautifulSoup
   
   url="http://www.hanbit.co.kr/store/books/full_book_list.html"
   r=requests.get(url)
   
   soup=BeautifulSoup(r.text, "html.parser")
   
   data1=soup.find("li")
   print(data1)
   print(data1.get_text())
   print('')
   
   data2=data1.next_sibling.next_sibling
   print(data2)
   print(data2.get_text())
   print('')
   
   data3=data2.next_sibling.next_sibling
   print(data3)
   print(data3.get_text())
   print('')
   ```

2. select

   - select를 사용해 지정한 곳의 데이터를 추출한다.

   ```python
   import requests
   from bs4 import BeautifulSoup
   
   url="https://movie.naver.com/movie/point/af/list.naver"
   r=requests.get(url)
   
   soup=BeautifulSoup(r.text, "html.parser")
   
   data=soup.find_all("td", attrs={"class": "title"})
   for i in data:
       print(i.a.get_text())
   
   for i in soup.select('td[class=title]'): #위와 동일한 동작. select를 이용
       print(i.a.text.strip())
   ```

   

---

### 실습

> 배운 것을 활용하여 문제를 풀어본다.



1. [한빛출판네트워크](https://www.hanbit.co.kr/store/books/full_book_list.html) 위 링크에서 책 이름들만 추출해 콘솔에 출력하기

   ``` python
   import requests
   from bs4 import BeautifulSoup
   
   url="https://www.hanbit.co.kr/store/books/full_book_list.html"
   r=requests.get(url)
   
   soup=BeautifulSoup(r.text, "html.parser")
   
   data=soup.find_all("td",attrs={"class":"left"})
   for i in data:
       if i.a: #a태그가 없는 데이터는 None으로 추출하기 때문에 데이터를 걸러줄 필요가 있음
           print(i.a.get_text())
   ```

2. [네이버 뉴스](https://search.naver.com/search.naver?where=nexearch&sm=tab_jum&query=%EC%BD%94%EB%A1%9C%EB%82%98) 위 링크에서 뉴스 제목들만 추출해 콘솔에 출력하기

   ```python
   import requests
   from bs4 import BeautifulSoup
   
   url="https://search.naver.com/search.naver?where=nexearch&sm=tab_jum&query=%EC%BD%94%EB%A1%9C%EB%82%98"
   r=requests.get(url)
   
   soup=BeautifulSoup(r.text, "html.parser")
   data=soup.select("a.news_tit")
   
   for i in data:
       print(i.get_text())
   ```

3. [네이버 영화](https://movie.naver.com/movie/point/af/list.naver) 위 링크에서 리뷰만 추출해 콘솔에 출력하기

   ```python
   import requests
   from bs4 import BeautifulSoup
   
   url="https://movie.naver.com/movie/point/af/list.naver"
   r=requests.get(url)
   
   soup=BeautifulSoup(r.text, "html.parser")
   data=soup.select("td.title")
   
   for i in data:
       # 항상 <br>뒤에 리뷰가 등장하기 때문에 next_sibling을 사용해 리뷰만 추출
       print(i.br.next_sibling.strip())
   ```

4. [네이버 금융](https://finance.naver.com/sise/sise_rise.naver) 위 링크에서 코스피의 종목명, 현재가, PER만 추출하여 콘솔에 출력하기

   ```python
   import requests
   from bs4 import BeautifulSoup
   
   def f(n,x): #입력값 x만큼 next_sibling하여 원하는 정보만 출력하는 함수
       for i in range(x):
           n=n.next_sibling
       return n.text
   
   url = 'https://finance.naver.com/sise/sise_rise.naver'
   r=requests.get(url)
   
   soup=BeautifulSoup(r.text,"html.parser")
   data=soup.select("td")
   
   for i in data:
       if i.a:
           print(f"종목명:{i.a.text},현재가:{f(i,2)},PER{f(i,18)}")
   ```