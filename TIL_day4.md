# 웹 크롤링과 스크레이핑 01



### 크롤링과 스크레이핑

> 크롤링과 스크레이핑은 같은 뜻으로 사용하는 경우가 많다.
>
> 하지만 두 단어는 약간의 차이가 있다.



1. 크롤링

   - 구글, 네이버와 같은 검색 포털에서 수행되며, 웹페이지의 정보를 수집하고 분류하여 데이터베이스화함.
   - 데이터를 수집하는 소프트웨어를 크롤러 혹은 봇이라고 부름.

   

2. 스크래핑

   - 넓은 의미로는 웹페이지의 정보를 수집하는 일련의 행위.
   - 좁은 의미로는 특정한 웹 페이지에서 원하는 데이터 일부를 가져오는 것



---

### 웹

> World Wid Web의 줄임말로 인터넷 상에서 동작하는 하나의 서비스이다.
>
> 흔히 혼동하는 인터넷은 컴퓨터 네트워크 통신망을 의미한다.



1. HTTP

   - 서버와 클라이언트는 프로토콜이라는 정해진 규약에 따라 통신하는데, HTTP는 HTML문서와같은 리소스들을 가져올 수 있도록 해주는 프로토콜이다.
   - 클라이언트의 요청이 있을 때만 서버가 응답하는 단방향 통신이다.
   - 서버는 클라이어트가 요청한 정보를 전송하고 곧바로 연결을 종료한다.

   

2. URL

   - 인터넷에서 자원의 위치를 나타낸다.
   - 자원들은 HTML페이지, CSS문서, 이미지 등이 될 수 있다.

   

3. HTTP 메서드-GET/POST

   - GET

     - 엽서(편지만 보낼 수 있다. 물건 X)
     - 주소와 함께 메시지를 남긴다.

   - POST

     - 택배(편지를 포함한 여러 물건도 보낼 수 있다.)
     - 주소와 함께 메시지나 물건도 보낼 수 있다.
     - 파일 업로드를 지원한다.

     

4. HTTP 헤더

   - HTTP요청/응답 시에 헤더 정보가 Key/Value 형식으로 세팅됨.
   - Body는 응답에만 존재

   

5. 웹페이지의 구성

   - HTML

     - 데이터를 구조적으로 표현하는 방법. 건물의 골격이라고 생각

   - CSS

     - HTML에 색을 입히거나 디자인을 변경해서 웹페이지를 예쁘게 꾸밈

   - 자바스크립트

     - 이벤트를 정의해서 웹페이지에 생명을 불어넣음.

     

---

### HTML

> Hypertext Markup Language



1. 하이퍼 텍스트

   - 비순차적으로 검색할 수 있는 문서를 의미
   - 일반적인 문서는 제공해주는 내용 그대로 하나씩 볼 수 있는 반면 하이퍼 텍스트는 링크를 클릭해서 다른 문서로의 이동을 지원

   

2. 마크업 언어

   - 태그와 같은 구분자를 사용해서 데이터의 구조를 기술
   - <태그></태그>형태로 사용. 슬래쉬가 포함된 태그를 종료 태그라고 함

   

3. 예시

   ```html
   <!DOCTYPE html>
   <html lang="en">
   <head>
       <meta charset="UTF-8">
       <title>안녕하세요</title>
   </head>
   <body>
   <h1>프로그램</h1>
   <form>
       id:<input type="text" name="입력장치1" placeholder="입력할 내용을 작성하시오" required><br><br>
       pw:<input type="password" name="입력장치2" placeholder="입력할 내용을 작성하시오" required><br><br>
   
   </form>
   <a>
       문서
   </a>
   
   </body>
   </html>
   ```

   

---

### JSON

> Javascript 객체 문법으로 구조화된 데이터를 표현하기 위한 문자 기반의 표준 포맷



- 웹 어플리케이션에서 데이터를 전송할 때 일반적으로 사용
- Javascript가 아니더라도 JSON을 읽고 쓸 수 있는 기능이 다수의 프로그래밍 환경에 존재
- JSON은 문자열 형태로 존재
- JSON파일 생성예시(python)

```python
import json

j_data1={
    "key1":"data1",
    "key2":"data2",
    "key3":"data3",
    "key4":[100,"data"]
}
j_data2={
    "key4":"data1",
    "key5":"data2",
    "key6":"data3",
    "key7":100
}

with open("data.json",'w') as f:
    json.dump(j_data1,f)
```



---

### urlib로 웹페이지 추출하기(python)

> 파이썬의 urlib.request 모듈을 사용하여 웹페이지를 추출해본다.



```python
from urllib.request import urlopen

f=urlopen("http://www.hanbit.co.kr")
print(f.read()) #HTTP 응답 본문(bytes 자료형)
print(f.status) #HTTP 응답 코드
print(f.getheader("Content-Type")) #HTTP 헤더
```



---

### HTTP 헤더에서 인코딩 방식 추출하기(python)

> HTTP 응답의 Content-Type 헤더를 참조하여 해당 페이지에 사용되고 있는 인코딩 방식을 알아낼 수 있다.



```python
import sys
from urllib.request import urlopen

f = urlopen('http://www.hanbit.co.kr/store/books/full_book_list.html')
encoding = f.info().get_content_charset(failobj="utf-8") #HTTP 헤더를 기반으로 인코딩 방식을 추출, 명시돼있지 않을 경우 utf-8사용
print('encoding:', encoding, file=sys.stderr) #인코딩 방식을 표준 오류에 출력
text = f.read().decode(encoding) #추출한 인코딩 방식으로 디코딩
print(text) #디코딩한 내용을 출력
```



---

### meta 태그에서 인코딩 방식 추출하기

> 일반적인 브라우저는 HTML내부의 meta태그 또는 응답 본문의 바이트열도 확인해서 최종적인 인코딩 방식을 결정하고 화면에 출력한다. HTTP헤더에서 추출하는 인코딩 정보가 항상 맞는 것은 아니기 때문에 meta태그를 통해 인코딩 방식을 추출하는 방법도 알아본다.



```python
import sys
import re
from urllib.request import urlopen

f = urlopen('http://www.hanbit.co.kr/store/books/full_book_list.html')
bytes_content = f.read()
scanned_text = bytes_content[:1024].decode('ascii', errors='replace') #응답 본문 앞부분 1024바이트를 ascii문자로 디코딩

match = re.search(r'charset=["\']?([\w-]+)', scanned_text) #디코딩한 문자열에서 정규 표현식으로 charset값을 추출

if match:
    encoding = match.group(1)
else: #charset이 명시돼 있지 않으면 utf-8사용
    encoding = 'utf-8'
print('encoding:', encoding, file=sys.stderr) #추출한 인코딩을 표준 오류에 출력
text = bytes_content.decode(encoding) #추출한 인코딩으로 다시 디코딩
print(text) #디코딩한 내용을 출력
```



---

### CSV

> CSV는 하나의 레코드를 한 줄에 나타내고, 각 줄의 값을 쉼표로 구분하는 텍스트 형식이다.
>
> 행과 열로 구성되는 2차원 데이터를 저장할 때 사용한다.



1. csv모듈을 사용해 CSV 형식으로 저장하기

```python


import csv

with open("data.csv",'w',newline='') as f:
    wd=csv.writer(f)
    wd.writerow(["data1","data2","data2"]) #단순히 문자열로 써짐. 키 값이 아님
    wd.writerows([[10,20],[10,20],[10,20],[10,20],[10,20]]) #데이터가 이미 1차원보다 크면 writerows를 쓰지만 1차원이라면 writerow를 반복문으로 사용해도 됨.
```

```python
#딕셔너리로 구성된 리스트를 CSV 형식으로 저장하기

import csv

with open("data.csv",'w',newline='') as f:
    wd = csv.DictWriter(f,["key1","key2","key3"]) #파일에 입력하는게 아니라 키 값을 설정하는 것
    wd.writeheader() #키 값이 헤더로 설정되고 파일 첫 줄에 입력됨.
    wd.writerow({"key1":10,"key2":20,"key3":30})
    wd.writerows(({"key1":10,"key2":20,"key3":30},
                 {"key1":10,"key2":20,"key3":30},
                 {"key1":10,"key2":20,"key3":30},
                 {"key1":10,"key2":20,"key3":30}))
    # 헤더로 설정한 키 중에 없는 키에 값을 넣으려고 하면 에러 발생. 딕셔너리처럼 키 값을 생성해서 값을 넣지 않음
```



---

### 데이터베이스(SQLite3)에 저장하기

> SQLite3는 파일 기반의 간단한 관계형 데이터베이스이다. SQL구문을 사용해 데이터를 읽고 쓸 수 있다.



```python
import sqlite3

conn = sqlite3.connect("data.db")
c=conn.cursor()
c.execute('DROP TABLE IF EXISTS cities') #cities 테이블이 이미 존재한다면 제거
c.execute('''
    CREATE TABLE cities (
        rank integer,
        city text,
        population integer
    )
''') #테이블 생성
#데이터 저장↓
c.execute('INSERT INTO cities VALUES (?, ?, ?)', (1, '상하이', 24150000))
c.execute('INSERT INTO cities VALUES (:rank, :city, :population)',
          {'rank': 2, 'city': '카라치', 'population': 23500000})
c.executemany('INSERT INTO cities VALUES (:rank, :city, :population)', [
    {'rank': 3, 'city': '베이징', 'population': 21516000},
    {'rank': 4, 'city': '텐진', 'population': 14722100},
    {'rank': 5, 'city': '이스탄불', 'population': 14160467},
])
c.executemany('INSERT INTO cities VALUES (?, ?, ?)', [(1, '상하이', 24150000),(1, '상하이', 24150000),(1, '상하이', 24150000),(1, '상하이', 24150000)])
conn.commit()

#데이터 출력↓
c.execute('SELECT * FROM cities') #저장한 데이터를 추출
for i in c.fetchall(): #쿼리의 결과를 fetchall메소드로 추출
    print(i)
    
conn.close()
```



---

### 파이썬으로 스크레이핑하는 흐름

> 스크레이핑하는 흐름은 추출->정규화->저장으로 흘러간다. 앞서 배웠던 내용들로 프로그램을 구성해본다.



```python
from urllib.request import urlopen
import re
from html import unescape
import sqlite3

def fetch(url):
    f = urlopen(url)
    encoding = f.info().get_content_charset(failobj="utf-8") #HTTP헤더로 인코딩 방식 추출
    html = f.read().decode(encoding) #추출한 인코딩 방식으로 디코딩
    return html #디코딩한 결과를 반환

def scrape(html):
    data=[]
    for i in re.findall(r'<td class="left"><a.*?</td>', html, re.DOTALL):
        url = re.search(r'<a href="(.*?)">',i).group(1) #도서의 URL을 추출
        url="http://www.hanbit.co.kr"+url
        title = re.sub(r'<.*?>', '',i) #태그를 제거해 도서의 제목 추출
        title = unescape(title)
        data.append({'url':url,"title":title})
    return data

def save(db,data):
    conn = sqlite3.connect(db)
    c=conn.cursor()
    c.execute('DROP TABLE IF EXISTS data') #data 테이블이 존재하면 제거
    c.execute('''
            CREATE TABLE data (
                title text,
                url text
            )
        ''') #테이블 생성
    c.executemany('INSERT INTO data VALUES (:title, :url)', data) #데이터 저장
    conn.commit()
    conn.close()

def print_db(db): #도서들과 주소를 출력
    conn = sqlite3.connect(db)
    c = conn.cursor()
    c.execute('SELECT * FROM data')
    for i in c.fetchall():
        print(i)

if __name__=="__main__":
    html=fetch("http://www.hanbit.co.kr/store/books/full_book_list.html")
    books=scrape(html)
    save('books.db', books)
    print_db('books.db')
```

