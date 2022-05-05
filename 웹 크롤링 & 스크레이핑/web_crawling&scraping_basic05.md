# 웹 크롤링과 스크레이핑 05

> 동적 크롤링에 대한 실습을 진행하면서 새로운 기능을 사용해본다.

<br/>

### 반응형 웹에 대한 동적 크롤링(구글뉴스)

> 구글뉴스의 경우는 스크롤을 끝까지 내리면 스크롤이 늘어나면서 새로운 내용이 추가되게 된다.
>
> 한 페이지에 정보를 모두 가져오려면 스크롤바가 더 이상 내려가지 않을 때까지 스크롤을 내려야 한다.

<br/>

```python
import time
from selenium import webdriver

b=webdriver.Chrome()
b.maximize_window() #크기 결정
b.implicitly_wait(10)

b.get("http://www.google.com") #구글 접속
b.implicitly_wait(10)

b.find_element_by_xpath('/html/body/div[1]/div[3]/form/div[1]/div[1]/div[1]/div/div[2]/input').send_keys("뉴스\n") #검색창에 뉴스 입력
b.implicitly_wait(10)

b.execute_script("window.scrollTo(0,500)") #스크롤 내리기
b.find_element_by_xpath('//*[@id="rso"]/div[2]/div/div/div[1]/div/a/h3').click() #구글 뉴스 클릭
b.implicitly_wait(10)

while True: #스크롤 계속 내리기
    info_n = b.execute_script("return document.body.scrollHeight")
    b.execute_script("window.scrollTo(0,document.body.scrollHeight)")
    time.sleep(2)

    next_n=b.execute_script("return document.body.scrollHeight")
    if info_n==next_n: #모든 내용을 담고 있는 최하단
        break
```

<br/>

---

### 검색사이트의 데이터를 페이지를 넘기며 가져오기

> 동적 크롤링으로 검색사이트의 데이터들을 각 페이지 별로 모두 모아본다.

<br/>

```python
from selenium import webdriver
from bs4 import BeautifulSoup

b=webdriver.Chrome()
b.implicitly_wait(10)

b.get("http://naver.com") #네이버 접속
b.implicitly_wait(10)

c=b.find_element_by_xpath('//*[@id="NM_FAVORITE"]/div[1]/ul[2]/li[2]/a').click() #뉴스 클릭
b.implicitly_wait(10)

c2=b.find_element_by_xpath('/html/body/section/header/div[2]/div/div/div[1]/div/div/ul/li[6]/a/span').click() #IT/과학 클릭
b.implicitly_wait(10)

c3=b.find_element_by_xpath('//*[@id="snb"]/ul/li[4]/a').click() #IT/일반 클릭
b.implicitly_wait(10)

for i in range(1,6):
    b.execute_script("window.scrollTo(0,document.body.scrollHeight)") #스크롤 맨 아래로 내리기
    b.implicitly_wait(10)
    html = b.page_source
    b.implicitly_wait(10)
    b.find_element_by_xpath(f'//*[@id="main_content"]/div[3]/a[{i}]').click() #i페이지 클릭
    b.implicitly_wait(10)

    s=BeautifulSoup(html, 'html.parser')
    data=s.select('dl')

    for i in data:
        if i.a:
            print(i.dd.previous_sibling.previous_sibling.a.text.strip()) #뉴스 제목
            print(i.dd.span.text.strip()) #뉴스 내용
            print('')
```

<br/>

<br/>

```python
from selenium import webdriver
from bs4 import BeautifulSoup

b=webdriver.Chrome()
b.implicitly_wait(10)

b.get("http://naver.com") #네이버 접속
b.implicitly_wait(10)

b.find_element_by_xpath('//*[@id="query"]').send_keys("2021 챔스\n") #2021 챔스 검색
b.implicitly_wait(10)

b.execute_script("window.scrollTo(0,document.body.scrollHeight)") #스크롤 맨 아래로
b.implicitly_wait(10)

b.find_element_by_xpath('//*[@id="main_pack"]/div[3]/div/div/a[2]').click() #2페이지 클릭

for i in range(3,7):
    b.implicitly_wait(10)
    b.execute_script("window.scrollTo(0,document.body.scrollHeight)")

    html=b.page_source
    b.find_element_by_xpath(f'//*[@id="main_pack"]/div[2]/div/div/a[{i}]').click()
    s=BeautifulSoup(html,'html.parser')
    data=s.select('a.link_tit')

    for i in data:
        print(i.text)
```

<br/>

---

### webdriver에 옵션을 추가해 더 간편하게 동적 크롤링하기

> 지금까지는 동적 크롤링을 진행하고 프로그램을 실행할 때 항상 웹 창이 띄워졌었다.
>
> 이번에는 옵션을 추가해 웹 창을 띄우지 않고도 정상적으로 크롤링이 작동하도록 해본다.

<br/>

```python
from selenium import webdriver
from bs4 import BeautifulSoup

op=webdriver.ChromeOptions()
op.headless=True #간접적으로 창을 열어서 확인
op.add_argument("window-size=1920x1080") #윈도우 사이즈 지정
op.add_argument('user-agent=Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/100.0.4896.75 Safari/537.36')
b=webdriver.Chrome(options=op) #설정한 옵션을 추가
b.maximize_window()
b.get("https://biz.chosun.com/it-science/ict/2022/04/04/KKSP7ZV6MFFLLINOIU4NN7QINE/?utm_source=naver&utm_medium=original&utm_campaign=biz")
s=BeautifulSoup(b.page_source, 'html.parser')
b.quit() #웹 종료
```

<br/>

<br/>

```python
from selenium import webdriver
from bs4 import BeautifulSoup
import csv

def f(a,li_d,file_name): #파일 저장 함수
    f=open(file_name, 'w', encoding='utf-8-sig', newline='')
    writer=csv.writer(f)
    writer.writerow(a)
    writer.writerows(li_d)
    f.close()

def print_f(file_name): #파일 출력 함수
    f=open(file_name, 'r', encoding='utf-8-sig', newline='')
    reader=csv.reader(f)
    skip=True
    for i in reader:
        if skip:
            skip = False
            continue
        print(i[0])
        print(i[1])
        print('')
    f.close()

b=webdriver.Chrome()
b.implicitly_wait(10)

b.get("http://naver.com") #네이버 접속
b.implicitly_wait(10)

b.find_element_by_xpath('//*[@id="query"]').send_keys("암호화폐\n") #암호화폐 검색
b.implicitly_wait(10)

b.find_element_by_xpath('//*[@id="lnb"]/div[1]/div/ul/li[2]/a').click() #뉴스 클릭
b.implicitly_wait(10)


li_d=[] #csv파일 만들때 사용할 리스트
a="제목", "내용"
for i in range(1,6):
    title = []  # 기사 제목 리스트
    contents = []  # 기사 내용 리스트
    b.execute_script("window.scrollTo(0,document.body.scrollHeight)")  # 스크롤 맨 아래로 내리기
    b.implicitly_wait(10)
    b.find_element_by_xpath(f'//*[@id="main_pack"]/div[2]/div/div/a[{i}]').click()  # i페이지 클릭
    b.implicitly_wait(10)
    html=b.page_source
    b.implicitly_wait(10)
    print(f"{i}페이지 진행중")

    s=BeautifulSoup(html,'html.parser')
    data1=s.find_all('a', attrs={"class":"news_tit"})
    data2=s.find_all('a', attrs={"class":"api_txt_lines dsc_txt_wrap"})

    for i in data1: #기사 제목 추출
        if i.text:
            title.append(i.text)

    for i in data2: #기사 내용 추출
        contents.append(i.text)

    for i in range(len(title)):  # 2차원 리스트 만들기
        li_d.append([title[i], contents[i]])

f(a,li_d,"news.csv")
print_f("news.csv")
```

- 옵션 추가에 더해서 모은 데이터를 csv파일로 저장하고 출력하는 프로그램이다.

<br/>

<br/>

```python
import csv
from selenium import webdriver
from bs4 import BeautifulSoup

def f(a,li_d,file_name): #파일 저장 함수
    f=open(file_name, 'w', encoding='utf-8-sig', newline='')
    writer=csv.writer(f)
    writer.writerow(a)
    writer.writerows(li_d)
    f.close()

def print_f(file_name): #파일 출력 함수
    f=open(file_name, 'r', encoding='utf-8-sig', newline='')
    reader=csv.reader(f)
    skip=True
    for i in reader:
        if skip:
            skip = False
            continue
        print(i[0])
        print(i[1])
        print('')
    f.close()

op=webdriver.ChromeOptions()
op.headless=True #웹 창을 띄우지 않고 진행
op.add_argument("window-size=1920x1080") #윈도우 사이즈 지정
op.add_argument('user-agent=Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/100.0.4896.75 Safari/537.36') #유저에이전트 설정
b=webdriver.Chrome(options=op) #크롬에 옵션 추가
b.maximize_window()
b.get("http://naver.com") #네이버 켜기
b.implicitly_wait(10)

b.find_element_by_xpath('//*[@id="query"]').send_keys("T1\n") #T1 검색
b.implicitly_wait(10)

b.find_element_by_xpath('//*[@id="lnb"]/div[1]/div/ul/li[2]/a').click() #뉴스 탭 클릭
b.implicitly_wait(10)

a="제목","내용"
file_name="news2.csv"
li_d=[] #최종적으로 csv파일에 저장할 리스트
for i in range(1,6):
    title=[]
    contents=[]
    b.execute_script("window.scrollTo(0,document.body.scrollHeight)") #스크롤 맨 아래로 내리기
    b.find_element_by_xpath(f'//*[@id="main_pack"]/div[2]/div/div/a[{i}]').click() #i페이지 클릭
    b.implicitly_wait(10)
    s=BeautifulSoup(b.page_source,'html.parser')
    print(f"{i}페이지 진행 중")

    data1=s.find_all('a', attrs={"class":"news_tit"}) #뉴스 제목 추출
    data2=s.find_all('a', attrs={"class":"api_txt_lines dsc_txt_wrap"}) #뉴스 내용 추출

    for i in data1: #뉴스 제목을 하나씩 꺼내서 title리스트에 추가
        if i.text:
            title.append(i.text)

    for i in data2: #뉴스 내용을 하나씩 꺼내서 contents리스트에 추가
        contents.append(i.text)

    for i in zip(title, contents): #뉴스 제목과 뉴스 내용을 각각 zip하여 li_d리스트에 추가
        li_d.append(i)

b.quit() #웹 닫기
f(a,li_d,file_name)
print_f(file_name)
```

