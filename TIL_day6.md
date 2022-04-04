# 웹 크롤링과 스크레이핑 03

> 02에서 했던 실습에서 얻어온 데이터를 다양한 파일형식으로 저장해본다.



### 크롤링한 데이터를 데이터베이스에 저장하기

```python
import sqlite3
import requests
from bs4 import BeautifulSoup

def f(i,x): #입력한 x만큼 next_sibling하여 텍스트로 추출하는 함수
    for n in range(x):
        i=i.next_sibling
    return i.text.strip()

def save_d(db,data): #db저장함수
    conn = sqlite3.connect(db)
    c=conn.cursor()
    c.execute('DROP TABLE IF EXISTS data')
    c.execute('''
        CREATE TABLE data (
            name text,
            cp text,
            ft text,
            fr text,
            tv text,
            ta text,
            bp text,
            ap text,
            mc text,
            PER text,
            ROE text
            )'''
        )
    c.executemany('INSERT INTO data VALUES (:name, :cp, :ft, :fr, :tv, :ta, :bp, :ap, :mc, :PER, :ROE)', data)
    conn.commit()
    conn.close()

def print_d(db): #db출력함수
    conn = sqlite3.connect(db)
    c = conn.cursor()
    c.execute('SELECT * FROM data')  # 저장한 데이터를 추출
    for i in c.fetchall():  # 쿼리의 결과를 fetchall메소드로 추출
        print(f"종목명: {i[0]} | 현재가: {i[1]} | 전일비: {i[2]} | 등락률: {i[3]} | 거래량: {i[4]} | 거래대금: {i[5]} | 매수호가: {i[6]} | 매도호가: {i[7]} | 시가총액: {i[8]} | PER: {i[9]} | ROE: {i[10]}")

def main():
    url="https://finance.naver.com/sise/sise_quant.naver?sosok=1"
    r=requests.get(url)

    soup=BeautifulSoup(r.text, "html.parser")
    data=soup.select("td")

    li_d=[] #딕셔너리를 저장할 리스트
    db="data.db" #데이터베이스 파일 이름
    for i in data:
        if i.a:
            li_d.append({"name":f"{i.a.text}", "cp":f"{f(i,2)}", "ft":f"{f(i,4)}", "fr":f"{f(i,6)}", "tv":f"{f(i,8)}"
                        , "ta":f"{f(i,10)}", "bp":f"{f(i,12)}", "ap":f"{f(i,14)}", "mc":f"{f(i,16)}", "PER":f"{f(i,18)}", "ROE":f"{f(i,20)}"})

    save_d(db,li_d)
    print_d(db)

if __name__=="__main__":
    main()
```



```python
#다른 주소로 실습

import sqlite3
import requests
from bs4 import BeautifulSoup

def save_d(db,data):
    conn=sqlite3.connect(db)
    c=conn.cursor()
    c.execute('DROP TABLE IF EXISTS data')
    c.execute('''
        CREATE TABLE data (
            rank text,
            team text,
            win text,
            lose text,
            winrate text,
            gl text
            )'''
        )
    c.executemany('INSERT INTO data VALUES (:rank, :team, :win, :lose, :winrate, :gl)', data)
    conn.commit()
    conn.close()

def print_d(db):
    conn=sqlite3.connect(db)
    c=conn.cursor()
    c.execute('SELECT * FROM data')
    print("순위 | 팀명 | 승 | 패 | 승률 | 득실")
    for i in c.fetchall():
        print(f"{i[0]} {i[1]} {i[2]} {i[3]} {i[4]} {i[5]}")
    conn.close()

def init_li():
    ranking=[]
    d_li=[]
    url = "https://search.naver.com/search.naver?where=nexearch&sm=tab_etc&mra=bjFE&pkid=475&os=25567154&qvt=0&query=2022%20LoL%20%EC%B1%94%ED%94%BC%EC%96%B8%EC%8A%A4%20%EC%BD%94%EB%A6%AC%EC%95%84%20%EC%8A%A4%ED%94%84%EB%A7%81%20%EC%A0%95%EA%B7%9C%EC%88%9C%EC%9C%84"
    r = requests.get(url)

    soup=BeautifulSoup(r.text, "html.parser")
    data=soup.select("td")

    for i in data:
        ranking.extend(i.text.split('    '))

    for i in range(0,len(ranking),6):
        d_li.append({"rank":ranking[i].lstrip(), "team":ranking[i+1].rstrip(), "win":ranking[i+2], "lose":ranking[i+3], "winrate":ranking[i+4], "gl":ranking[i+5]})

    return d_li

def main():
    init_li()
    save_d("data.db",init_li())
    print_d("data.db")

if __name__=="__main__":
    main()
```



```python
#다른 주소로 실습

import sqlite3
import requests
from bs4 import BeautifulSoup

def save_d(db,data):
    conn = sqlite3.connect(db)
    c=conn.cursor()
    c.execute('DROP TABLE IF EXISTS data')
    c.execute('''
        CREATE TABLE data (
            title text,
            reviewer text,
            review text
            )'''
        )
    c.executemany('INSERT INTO data VALUES (:title, :reviewer, :review)', data)
    conn.commit()
    conn.close()

def print_d(db):
    conn = sqlite3.connect(db)
    c = conn.cursor()
    c.execute('SELECT * FROM data')
    for i in c.fetchall():
        print(f"{i[0]}\n{i[2]}\t{i[1]}\n")
    conn.close()

def init_li():
    title = [] #영화 제목 데이터를 추출해서 넣을 리스트
    reviewer = [] #리뷰한 사람의 닉네임의 데이터를 추출해서 넣을 리스트
    review = [] #리뷰 내용 데이터를 추출해서 넣을 리스트
    m_dic_li = [] #얻은 데이터들로 만든 딕셔너리를 넣을 리스트
    url = "https://movie.naver.com/movie/point/af/list.naver?&page=10"
    r = requests.get(url)

    soup = BeautifulSoup(r.text, "html.parser")
    data1=soup.select("a.movie.color_b") #영화 제목
    data2=soup.select("a.author") #리뷰어
    data3=soup.find_all("td", "title") #리뷰

    for i in data1:
        title.append(i.text)

    for i in data2:
        reviewer.append(i.text)

    for i in data3:
        review.append(i.br.next_sibling.strip())

    for i in range(len(data1)):
        m_dic_li.append({"title":title[i], "reviewer":reviewer[i], "review":review[i]})

    return m_dic_li

def main():
    init_li()
    save_d("data.db", init_li())
    print_d("data.db")

if __name__=="__main__":
    main()
```



### 크롤링 시 시간간격 주기

> 짧은 시간에 많은 횟수의 크롤링을 하면(접속시도를 하면) 접속을 차단당할 수 있기 때문에
>
> 시간간격을 주고 크롤링한다.



```python
import time
from random import randint
import csv
import requests #수집
from bs4 import BeautifulSoup #정리

url="https://movie.naver.com/movie/point/af/list.naver?&page=" #파일이름

#파일 내용 정리
title="영화명","평점","리뷰"
f=open("save.csv", 'w', encoding='utf-8-sig', newline='') #한글 저장하려면 utf-8-sig로 인코딩할 것
writer=csv.writer(f)
writer.writerow(title)
in_data=[]

#data 수집
for page in range(1,6):
    print(f"{page}페이지 크롤링 중")
    r=requests.get(url+str(page))
    r.raise_for_status() #접속 상태 확인 (접속 코드 200 아닐시 예외 발생)
    soup=BeautifulSoup(r.text,"html.parser")
    data=soup.find_all("td",attrs={"class":"title"})
    
    #파일 정리
    for i in data:
        if i.a:
            #단일 입력
            #in_data=[i.a.text, i.em.text, i.br.next_sibling.strip()]
            #writer.writerow(in_data)
            in_data.append([i.a.text, i.em.text, i.br.next_sibling.strip()])
    time.sleep(randint(5,10)) #sleep을 추가하지 않으면 IP밴 당할 수도 있음

#저장
writer.writerows(in_data)
```



```python
#실습

import time
import csv
import requests
from bs4 import BeautifulSoup

url='https://news.naver.com/main/list.naver?mode=LS2D&sid2=228&sid1=105&mid=shm&date=20220404&page='
headers={"User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/84.0.4147.89 Safari/537.36"}
li_d=[] #csv파일에 저장할 리스트

def init_li_d(): #li_d를 초기화하는 함수
    for page in range(1, 6): #5페이지까지
        title=[] #뉴스 제목을 저장하는 리스트
        sh=[] #뉴스 소제목을 저장하는 리스트
        print(f"{page}페이지 크롤링 중")
        r=requests.get(url + str(page), headers=headers)
        r.raise_for_status()
        soup=BeautifulSoup(r.text, "html.parser")
        data1=soup.select("dt") #뉴스 제목
        data2=soup.find_all("span", "lede") #뉴스 소제목

        for i in data1:
            if i.text.strip()!='' and i.text.strip()!="동영상기사": #뉴스제목 이외의 것들을 제외
                title.append(i.text.strip())

        for i in data2:
            sh.append(i.text.strip())

        for i in range(len(title)):
            li_d.append([title[i], sh[i]])

        time.sleep(5) #크롤링은 5초 간격으로

def f(data):
    f=open("data.csv", 'w', encoding='utf-8-sig', newline='')
    writer=csv.writer(f)
    writer.writerows(data)
    f.close()

def main():
    init_li_d()
    f(li_d)

if __name__=="__main__":
    main()
```

