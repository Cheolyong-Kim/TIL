# 롤 인벤 msi 검색 결과 크롤링 (아주 간단하게)

<br/>

![j1](https://github.com/Cheolyong-Kim/TIL/blob/master/%EC%8B%A4%EC%8A%B5/%EB%A1%A4%20%EC%9D%B8%EB%B2%A4%20msi%20%EA%B2%80%EC%83%89%20%EA%B2%B0%EA%B3%BC%20%ED%81%AC%EB%A1%A4%EB%A7%81(%EC%95%84%EC%A3%BC%20%EA%B0%84%EB%8B%A8%ED%95%98%EA%B2%8C)/image/j1.png?raw=true)

- 필요한 패키지들을 불러온다.
- tqdm 패키지는 반복문 실행 시 경과 정도를 눈으로 볼 수 있게 해준다.
- warnings 패키지를 통해 발생하는 워닝을 보이지 않게 해준다.

<br/>

![j2](https://github.com/Cheolyong-Kim/TIL/blob/master/%EC%8B%A4%EC%8A%B5/%EB%A1%A4%20%EC%9D%B8%EB%B2%A4%20msi%20%EA%B2%80%EC%83%89%20%EA%B2%B0%EA%B3%BC%20%ED%81%AC%EB%A1%A4%EB%A7%81(%EC%95%84%EC%A3%BC%20%EA%B0%84%EB%8B%A8%ED%95%98%EA%B2%8C)/image/j2.png?raw=true)

- query_text에는 검색할 단어를 저장해둔다.
- 크롬 드라이버를 만들고 url을 지정하여 해당 링크를 열어준다.

<br/>

![j3](https://github.com/Cheolyong-Kim/TIL/blob/master/%EC%8B%A4%EC%8A%B5/%EB%A1%A4%20%EC%9D%B8%EB%B2%A4%20msi%20%EA%B2%80%EC%83%89%20%EA%B2%B0%EA%B3%BC%20%ED%81%AC%EB%A1%A4%EB%A7%81(%EC%95%84%EC%A3%BC%20%EA%B0%84%EB%8B%A8%ED%95%98%EA%B2%8C)/image/j3.png?raw=true)

- 사이트의 html문서를 확인하여 검색하는 부분의 위치를 파악한다.
- name 속성의 'keyword'를 활용할 것이다.

<br/>

```python
element = driver.find_element_by_name('keyword')
element.send_keys(query_txt)
element.submit()
time.sleep(1)
```

- find_element_by_name메소드에 'keyword'를 입력하여 검색바 위치를 찾고 query_txt를 입력하고 검색한다.

<br/>

![j5](https://github.com/Cheolyong-Kim/TIL/blob/master/%EC%8B%A4%EC%8A%B5/%EB%A1%A4%20%EC%9D%B8%EB%B2%A4%20msi%20%EA%B2%80%EC%83%89%20%EA%B2%B0%EA%B3%BC%20%ED%81%AC%EB%A1%A4%EB%A7%81(%EC%95%84%EC%A3%BC%20%EA%B0%84%EB%8B%A8%ED%95%98%EA%B2%8C)/image/j5.png?raw=true)

- 검색 후 게시판 더보기를 클릭하기 위해 링크를 확인한다.

<br/>

![j4](https://github.com/Cheolyong-Kim/TIL/blob/master/%EC%8B%A4%EC%8A%B5/%EB%A1%A4%20%EC%9D%B8%EB%B2%A4%20msi%20%EA%B2%80%EC%83%89%20%EA%B2%B0%EA%B3%BC%20%ED%81%AC%EB%A1%A4%EB%A7%81(%EC%95%84%EC%A3%BC%20%EA%B0%84%EB%8B%A8%ED%95%98%EA%B2%8C)/image/j4.png?raw=true)

- find_element_by_link_text를 통해 링크를 가지고 있는 텍스트 위치를 정할 수 있다.
- '게시판 더보기'를 입력하고 클릭해준다.

<br/>

![j7](https://github.com/Cheolyong-Kim/TIL/blob/master/%EC%8B%A4%EC%8A%B5/%EB%A1%A4%20%EC%9D%B8%EB%B2%A4%20msi%20%EA%B2%80%EC%83%89%20%EA%B2%B0%EA%B3%BC%20%ED%81%AC%EB%A1%A4%EB%A7%81(%EC%95%84%EC%A3%BC%20%EA%B0%84%EB%8B%A8%ED%95%98%EA%B2%8C)/image/j7.png?raw=true)

- 최신 글들을 위주로 크롤링하기 위해 '최신순' 위치 확인

<br/>

![j6](https://github.com/Cheolyong-Kim/TIL/blob/master/%EC%8B%A4%EC%8A%B5/%EB%A1%A4%20%EC%9D%B8%EB%B2%A4%20msi%20%EA%B2%80%EC%83%89%20%EA%B2%B0%EA%B3%BC%20%ED%81%AC%EB%A1%A4%EB%A7%81(%EC%95%84%EC%A3%BC%20%EA%B0%84%EB%8B%A8%ED%95%98%EA%B2%8C)/image/j6.png?raw=true)

- '게시판 더보기'와 같은 동작을 해준다.

<br/>

![j9](https://github.com/Cheolyong-Kim/TIL/blob/master/%EC%8B%A4%EC%8A%B5/%EB%A1%A4%20%EC%9D%B8%EB%B2%A4%20msi%20%EA%B2%80%EC%83%89%20%EA%B2%B0%EA%B3%BC%20%ED%81%AC%EB%A1%A4%EB%A7%81(%EC%95%84%EC%A3%BC%20%EA%B0%84%EB%8B%A8%ED%95%98%EA%B2%8C)/image/j9.png?raw=true)

- 게시물의 링크를 알아내기 위해 위치를 확인한다.
- class 'item' 하위에 class 'name'에 존재하는 것을 확인했다.

<br/>

![j8](https://github.com/Cheolyong-Kim/TIL/blob/master/%EC%8B%A4%EC%8A%B5/%EB%A1%A4%20%EC%9D%B8%EB%B2%A4%20msi%20%EA%B2%80%EC%83%89%20%EA%B2%B0%EA%B3%BC%20%ED%81%AC%EB%A1%A4%EB%A7%81(%EC%95%84%EC%A3%BC%20%EA%B0%84%EB%8B%A8%ED%95%98%EA%B2%8C)/image/j8.png?raw=true)

- url_list와 title_list를 미리 만들어 둔다. 각각 url과 게시글 제목을 저장해둘 리스트이다.
- css_selector로 해당 위치의 정보를 가져온다.

<br>

![j10](https://github.com/Cheolyong-Kim/TIL/blob/master/%EC%8B%A4%EC%8A%B5/%EB%A1%A4%20%EC%9D%B8%EB%B2%A4%20msi%20%EA%B2%80%EC%83%89%20%EA%B2%B0%EA%B3%BC%20%ED%81%AC%EB%A1%A4%EB%A7%81(%EC%95%84%EC%A3%BC%20%EA%B0%84%EB%8B%A8%ED%95%98%EA%B2%8C)/image/j10.png?raw=true)

- 해당 정보에서 href속성(링크)를 출력해본 결과

<br/>

![j11](https://github.com/Cheolyong-Kim/TIL/blob/master/%EC%8B%A4%EC%8A%B5/%EB%A1%A4%20%EC%9D%B8%EB%B2%A4%20msi%20%EA%B2%80%EC%83%89%20%EA%B2%B0%EA%B3%BC%20%ED%81%AC%EB%A1%A4%EB%A7%81(%EC%95%84%EC%A3%BC%20%EA%B0%84%EB%8B%A8%ED%95%98%EA%B2%8C)/image/j11.png?raw=true)

- for문을 반복하여 가져온 정보들에서 url과 제목을 추출한다.

<br/>

![j12](https://github.com/Cheolyong-Kim/TIL/blob/master/%EC%8B%A4%EC%8A%B5/%EB%A1%A4%20%EC%9D%B8%EB%B2%A4%20msi%20%EA%B2%80%EC%83%89%20%EA%B2%B0%EA%B3%BC%20%ED%81%AC%EB%A1%A4%EB%A7%81(%EC%95%84%EC%A3%BC%20%EA%B0%84%EB%8B%A8%ED%95%98%EA%B2%8C)/image/j12.png?raw=true)

![j13](https://github.com/Cheolyong-Kim/TIL/blob/master/%EC%8B%A4%EC%8A%B5/%EB%A1%A4%20%EC%9D%B8%EB%B2%A4%20msi%20%EA%B2%80%EC%83%89%20%EA%B2%B0%EA%B3%BC%20%ED%81%AC%EB%A1%A4%EB%A7%81(%EC%95%84%EC%A3%BC%20%EA%B0%84%EB%8B%A8%ED%95%98%EA%B2%8C)/image/j13.png?raw=true)

- 가져온 게시글 url 목록과 제목 목록이다.

<br/>

![j14](https://github.com/Cheolyong-Kim/TIL/blob/master/%EC%8B%A4%EC%8A%B5/%EB%A1%A4%20%EC%9D%B8%EB%B2%A4%20msi%20%EA%B2%80%EC%83%89%20%EA%B2%B0%EA%B3%BC%20%ED%81%AC%EB%A1%A4%EB%A7%81(%EC%95%84%EC%A3%BC%20%EA%B0%84%EB%8B%A8%ED%95%98%EA%B2%8C)/image/j14.png?raw=true)

- 추출한 게시글 url과 제목을 판다스를 활용하여 데이터 프레임화 해준다.

<br/>

![j15](https://github.com/Cheolyong-Kim/TIL/blob/master/%EC%8B%A4%EC%8A%B5/%EB%A1%A4%20%EC%9D%B8%EB%B2%A4%20msi%20%EA%B2%80%EC%83%89%20%EA%B2%B0%EA%B3%BC%20%ED%81%AC%EB%A1%A4%EB%A7%81(%EC%95%84%EC%A3%BC%20%EA%B0%84%EB%8B%A8%ED%95%98%EA%B2%8C)/image/j15.png?raw=true)

- 데이터 프레임을 csv 파일로 저장한다.
- 처음부터 끝까지 크롤링하여 데이터를 가져와도 되지만, 링크를 저장한 뒤 그 링크로 다시 크롤링하면 코드로 짜기 편하고 동적 크롤링이 아닌 이상 브라우저가 여러개 켜지는 부담도 없기 때문에 이 방식으로 진행한다.

<br/>

![j16](https://github.com/Cheolyong-Kim/TIL/blob/master/%EC%8B%A4%EC%8A%B5/%EB%A1%A4%20%EC%9D%B8%EB%B2%A4%20msi%20%EA%B2%80%EC%83%89%20%EA%B2%B0%EA%B3%BC%20%ED%81%AC%EB%A1%A4%EB%A7%81(%EC%95%84%EC%A3%BC%20%EA%B0%84%EB%8B%A8%ED%95%98%EA%B2%8C)/image/j16.png?raw=true)

- 저장해놨던 링크 파일을 가져온다.

<br/>

![j17](https://github.com/Cheolyong-Kim/TIL/blob/master/%EC%8B%A4%EC%8A%B5/%EB%A1%A4%20%EC%9D%B8%EB%B2%A4%20msi%20%EA%B2%80%EC%83%89%20%EA%B2%B0%EA%B3%BC%20%ED%81%AC%EB%A1%A4%EB%A7%81(%EC%95%84%EC%A3%BC%20%EA%B0%84%EB%8B%A8%ED%95%98%EA%B2%8C)/image/j17.png?raw=true)

- url 파일의 첫번째 링크를 url에 저장
- 그 링크를 크롬 브라우저로 열어준다.

<br/>

![j19](https://github.com/Cheolyong-Kim/TIL/blob/master/%EC%8B%A4%EC%8A%B5/%EB%A1%A4%20%EC%9D%B8%EB%B2%A4%20msi%20%EA%B2%80%EC%83%89%20%EA%B2%B0%EA%B3%BC%20%ED%81%AC%EB%A1%A4%EB%A7%81(%EC%95%84%EC%A3%BC%20%EA%B0%84%EB%8B%A8%ED%95%98%EA%B2%8C)/image/j19.png?raw=true)

- 게시글에서 제목을 추출하기 위해 제목이 위치하는 곳을 확인한다.

<br/>

![j18](https://github.com/Cheolyong-Kim/TIL/blob/master/%EC%8B%A4%EC%8A%B5/%EB%A1%A4%20%EC%9D%B8%EB%B2%A4%20msi%20%EA%B2%80%EC%83%89%20%EA%B2%B0%EA%B3%BC%20%ED%81%AC%EB%A1%A4%EB%A7%81(%EC%95%84%EC%A3%BC%20%EA%B0%84%EB%8B%A8%ED%95%98%EA%B2%8C)/image/j18.png?raw=true)

- 게시글 별 데이터를 딕셔너리로 저장할 target_info와 그 딕셔너리를 저장할 dict 딕셔너리를 미리 정의해둔다.
- css_selector로 확인한 위치의 정보를 가져온다.
- tit.text를 통해 제목을 가져온다

<br/>

![j21](https://github.com/Cheolyong-Kim/TIL/blob/master/%EC%8B%A4%EC%8A%B5/%EB%A1%A4%20%EC%9D%B8%EB%B2%A4%20msi%20%EA%B2%80%EC%83%89%20%EA%B2%B0%EA%B3%BC%20%ED%81%AC%EB%A1%A4%EB%A7%81(%EC%95%84%EC%A3%BC%20%EA%B0%84%EB%8B%A8%ED%95%98%EA%B2%8C)/image/j21.png?raw=true)

- 작성자 닉네임을 추출하기 위해 해당 위치를 확인한다.

<br/>

![j20](https://github.com/Cheolyong-Kim/TIL/blob/master/%EC%8B%A4%EC%8A%B5/%EB%A1%A4%20%EC%9D%B8%EB%B2%A4%20msi%20%EA%B2%80%EC%83%89%20%EA%B2%B0%EA%B3%BC%20%ED%81%AC%EB%A1%A4%EB%A7%81(%EC%95%84%EC%A3%BC%20%EA%B0%84%EB%8B%A8%ED%95%98%EA%B2%8C)/image/j20.png?raw=true)

- 제목을 추출할 때와 동일하게 동작

<br/>

![j23](https://github.com/Cheolyong-Kim/TIL/blob/master/%EC%8B%A4%EC%8A%B5/%EB%A1%A4%20%EC%9D%B8%EB%B2%A4%20msi%20%EA%B2%80%EC%83%89%20%EA%B2%B0%EA%B3%BC%20%ED%81%AC%EB%A1%A4%EB%A7%81(%EC%95%84%EC%A3%BC%20%EA%B0%84%EB%8B%A8%ED%95%98%EA%B2%8C)/image/j23.png?raw=true)

- 게시 날짜를 추출하기 위해 위치를 확인

<br/>

![j22](https://github.com/Cheolyong-Kim/TIL/blob/master/%EC%8B%A4%EC%8A%B5/%EB%A1%A4%20%EC%9D%B8%EB%B2%A4%20msi%20%EA%B2%80%EC%83%89%20%EA%B2%B0%EA%B3%BC%20%ED%81%AC%EB%A1%A4%EB%A7%81(%EC%95%84%EC%A3%BC%20%EA%B0%84%EB%8B%A8%ED%95%98%EA%B2%8C)/image/j22.png?raw=true)

- 제목을 추출했을 때와 동일하게 동작

<br/>

![j25](https://github.com/Cheolyong-Kim/TIL/blob/master/%EC%8B%A4%EC%8A%B5/%EB%A1%A4%20%EC%9D%B8%EB%B2%A4%20msi%20%EA%B2%80%EC%83%89%20%EA%B2%B0%EA%B3%BC%20%ED%81%AC%EB%A1%A4%EB%A7%81(%EC%95%84%EC%A3%BC%20%EA%B0%84%EB%8B%A8%ED%95%98%EA%B2%8C)/image/j25.png?raw=true)

- 게시글 본문을 추출하기 위해 위치를 확인한다.

<br/>

![j24](https://github.com/Cheolyong-Kim/TIL/blob/master/%EC%8B%A4%EC%8A%B5/%EB%A1%A4%20%EC%9D%B8%EB%B2%A4%20msi%20%EA%B2%80%EC%83%89%20%EA%B2%B0%EA%B3%BC%20%ED%81%AC%EB%A1%A4%EB%A7%81(%EC%95%84%EC%A3%BC%20%EA%B0%84%EB%8B%A8%ED%95%98%EA%B2%8C)/image/j24.png?raw=true)

<br/>

![j26](https://github.com/Cheolyong-Kim/TIL/blob/master/%EC%8B%A4%EC%8A%B5/%EB%A1%A4%20%EC%9D%B8%EB%B2%A4%20msi%20%EA%B2%80%EC%83%89%20%EA%B2%B0%EA%B3%BC%20%ED%81%AC%EB%A1%A4%EB%A7%81(%EC%95%84%EC%A3%BC%20%EA%B0%84%EB%8B%A8%ED%95%98%EA%B2%8C)/image/j26.png?raw=true)

- 게시글 본문 추출 결과

<br/>

![j27](https://github.com/Cheolyong-Kim/TIL/blob/master/%EC%8B%A4%EC%8A%B5/%EB%A1%A4%20%EC%9D%B8%EB%B2%A4%20msi%20%EA%B2%80%EC%83%89%20%EA%B2%B0%EA%B3%BC%20%ED%81%AC%EB%A1%A4%EB%A7%81(%EC%95%84%EC%A3%BC%20%EA%B0%84%EB%8B%A8%ED%95%98%EA%B2%8C)/image/j27.png?raw=true)

- 가져온 본문들을 스페이스바를 기준으로 합쳐줌
- 해당 게시글에는 본문이 하나이기 때문에 한 번 동작함

<br/>

![j28](https://github.com/Cheolyong-Kim/TIL/blob/master/%EC%8B%A4%EC%8A%B5/%EB%A1%A4%20%EC%9D%B8%EB%B2%A4%20msi%20%EA%B2%80%EC%83%89%20%EA%B2%B0%EA%B3%BC%20%ED%81%AC%EB%A1%A4%EB%A7%81(%EC%95%84%EC%A3%BC%20%EA%B0%84%EB%8B%A8%ED%95%98%EA%B2%8C)/image/j28.png?raw=true)

- 추출한 데이터들을 각자 target_info 딕셔너리에 추가
- 만든 target_info 딕셔너리를 dict에 추가

<br/>

```python
dict = {}

number = 10
chrome_path = chromedriver_autoinstaller.install()

for i in tqdm_notebook(range(0, number)):
    url = url_load['url'][i]
    driver = webdriver.Chrome(chrome_path)
    driver.get(url)
    
    try:
        target_info = {}
        
        overlays = '.articleTitle'
        tit = driver.find_element_by_css_selector(overlays)
        title = tit.text
        
        overlays = '.articleWriter'
        nick = driver.find_element_by_css_selector(overlays)
        nickname = nick.text
        
        overlays = '.articleDate'
        date = driver.find_element_by_css_selector(overlays)
        datetime = date.text
        
        overlays = ".contentBody"                                 
        contents = driver.find_elements_by_css_selector(overlays)
        
        content_list = []
        for content in contents:
            content_list.append(content.text)

        content_str = ' '.join(content_list)
        
        target_info['title'] = title
        target_info['nickname'] = nickname
        target_info['datetime'] = datetime
        target_info['content'] = content_str
        
        dict[i] = target_info
        time.sleep(1)
        
        driver.close() 
    
    except:
        print('error')
        driver.close()
        time.sleep(1)
        continue
```

- 위 과정을 for문 하나로 합친 코드

<br/>

![j29](https://github.com/Cheolyong-Kim/TIL/blob/master/%EC%8B%A4%EC%8A%B5/%EB%A1%A4%20%EC%9D%B8%EB%B2%A4%20msi%20%EA%B2%80%EC%83%89%20%EA%B2%B0%EA%B3%BC%20%ED%81%AC%EB%A1%A4%EB%A7%81(%EC%95%84%EC%A3%BC%20%EA%B0%84%EB%8B%A8%ED%95%98%EA%B2%8C)/image/j29.png?raw=true)

- 생성한 dict 딕셔너리를 데이터 프레임화 해준다.
- 만든 데이터 프레임을 csv 파일로 저장하면 게시글 크롤링 끝

<br/>

---

단순히 1페이지를 추출하기도 했고 추출한 데이터들도 전처리하지 않아서 제대로 된 크롤링은 아니지만

다른 사이트에서 크롤링을 할 때도 이런 흐름으로 동작한다는 것을 기록하는 목적으로 실습을 진행했다.

