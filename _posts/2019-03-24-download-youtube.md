---
layout: post
comments: true
title: "python으로 노가다 자동화하기_수 백개의 Youtube 영상을 손 안대고 mp3파일로 저장하는 방법"
categories:
  - Study Note
tags:
    - python
    - youtube
    - mp4-to-mp3
    - crawling
    - requests
    - bs4
---

## Intro
---
머신러닝 프로젝트를 진행하면서 약 600개 이상의 동영상에서 음성데이터를 따야할 일이 생겼다.

누구에게 제공하거나 상업적으로 이용하려는 목적이 전혀 아니기 때문에 유튜브에서 음성파일을 `.mp3`로 추출하기로 했다.

따야할 음성의 제목은 엑셀 파일로 정리해놓은 상태고 그냥 노가다로 일일이 다운받아도 될 일이긴 했다...

근데 600개를..언제 다..??

개발 공부를 하고 있는데 노가다로 작업한다는 건 문제가 좀 있지 않을까..`자동화`할 수 있진 않을까?

라는 생각에 해결해야하는 문제를 모두 `자동화` 할 수 있는 코드를 작성해보기로 했다. 

( 내 시간은 소중하기 때문에.. 하지만 코드 작성에 네 시간 걸린건 안비밀 )

나 대신 코드가 모든 일을 처리해 주도록 해보자!

<br>

<br>

## 해결해야 하는 문제
---

일단 자동화 해야할 문제들은 아래와 같았다.

1. **엑셀 파일에 적어놓은 수 백개의 동영상 title 읽어오기**
2. **가져온 title에 해당하는 유튜브 동영상을 mp4파일로  다운받기**
3. **다운받은 mp4 파일을 원하는 format_name을 가진 mp3 파일로 변환하기**
4. **mp4 파일 지우기 (난 mp3만 필요하므로)**

<br>

<br>

## .xlsx 파일에서 데이터 읽어오기
---

일단 첫 번째 문제부터 해결해보자.

`python으로 엑셀파일 읽기` 검색 후 **`openpyxl`** 라는 라이브러리가 있다는 것을 알게되었다.

현재 내 엑셀 파일 `데이터셋_soft.xlsx` 에는 아래와 같은 형식으로 **2개**의 col과 header포함 **151개**의 row들이 저장되어있다.

| id | title   |
|----|---------|
| 1  | title_1 |
| 2  | title_2 |
| 3  | title_3 |

<br>

이렇게 엑셀로 저장되어있는 데이터들을 아래와 같이 가져왔다.

<br>

```
from openpyxl import load_workbook # pip install openpyxl

# 해당 경로에서 엑셀 파일 가져오기
load_wb = load_workbook("C:/Users/kwons/Desktop/데이터셋_soft.xlsx", data_only=True)
# 시트 이름으로 불러오기
load_ws = load_wb['Sheet1']

print('헤더를 제외한 엑셀의 모든 행과 열 저장')
all_values = []
for row in load_ws['A2':'B151']:
    row_value = []
    for cell in row:
        row_value.append(cell.value)
    all_values.append(row_value)

#test
print(all_values)
print(all_values[0][0], all_values[0][1]) # 각각 id->new_filename에 이용, title -> query에 이용
print(len(all_values)) # 150

```

<br>

헤더를 제외하기 위해서 셀 **`A2`**(=1) 부터 **`B151`**(=title_150)까지 `all_values`에 저장했다.

`all_values`에는 `[[1, title_1], [2, title_2], [3, title_3], ... ,[150, title_150]]` 이렇게 이차원 배열이 저장된다.

*참고 >> `A`와 `B`는 각각 1열, 2열이고 알파벳 뒤에 붙은 숫자는 행이다. 예를 들어 `A1`의 경우 1행 1열인 `id`이며 `B3`의 경우 3행 2열인 `title_2` 이다.

<br>

<br>

## 유튜브 동영상을 .mp4 파일로 다운 받기
---
이제 엑셀파일의 데이터들은 잘 저장해놨고, 두 번쨰 문제를 해결해보자.

`all_values`에 저장된 **title**을 가진 유튜브 동영상을 검색해서 가져와야한다. 

그럼 유튜브에서 검색을 하면 검색어가 어떤 **param**으로 어떤 **url**에 request가 보내지는지 확인해야 한다.

확인해보면 **`https://www.youtube.com/results`** 의 url로, 검색어는 **`search_query`** 라는 param으로  query되는 것을 알 수 있다.

<img src="/assets/images/190324/youtube.JPG">

<br>

따라서 url로 request해주는 라이브러리 `requests`를 사용해서 유튜브에 검색해야 하는 `title`로 param을 넘기고, 그 결과를 `response`로 받았다.

그리고 크롤링해주는 라이브러리 `bs4`를 사용해서 `response`로 받은 html을 파싱해서 `watch_url`을 받아왔다!

`watch_url`은 해당 동영상을 mp4로 저장할 때 그 url을 파라미터로 넘겨야해서 필요하다. 

나는 **postman**을 통해 `GET`요청을 보낸 후 천천히 찾아보았다... 

노랑색으로 하이라이트 친 부분이 검색결과로 가장 먼저 나오는 동영상의 `watch_url`이다. 이걸 가져와야한다.

<img src="/assets/images/190324/youtube2.JPG">

<br>

```
import requests # pip install requests
from bs4 import BeautifulSoup # pip install bs4

URL = 'https://www.youtube.com/results'
for row in range(len(all_values)): # 0~149 까지 엑셀파일의 모든 노래 loop
    # HTTP request
    params = {'search_query': all_values[row][1] } # 엑셀파일의 'title'에 해당하는 문자열로 query
    response = requests.get(URL, params=params)

    # parsing
    html = response.text
    soup = BeautifulSoup(html, 'html.parser')
    watch_url = soup.find_all(class_='yt-uix-sessionlink spf-link')[0]['href']
```

<br>

위와 같이 `soup.find_all(class_='yt-uix-sessionlink spf-link')[0]['href']` 로 첫 번째 동영상의 `watch_url`을 가져왔고!

이제 드디어 동영상의 .mp4 파일을 다운 받을 준비가 됐다ㅜㅜ

**`pytube`** 라는 라이브러리를 사용해서 유튜브 동영상을 mp4파일로 `C:/Users/kwons/dataset_soft`에 저장했다.

<br>

```
import pytube # pip install pytube
 
...

    # 유튜브 가져오기
    youtube = pytube.YouTube("https://www.youtube.com" + watch_url) # 동영상 url
    videos = youtube.streams.all()

    
    # 다운받을 수 있는 스트리밍 종류 -> 0번이 mp4
    for i in range(len(videos)) :
        print(i,":", videos[i])
    

    parent_dir = "C:/Users/kwons/dataset_soft" # 저장할 파일 경로
    videos[0].download(parent_dir) # mp4로 다운로드
```
<br>

( 일단 들여쓰기가 전체적으로 어떻게 되는지 모르겠을 것이다. 포스팅 하단에 전체 코드를 올릴 것이므로 조금만 인내심을 가져주시길 바랍니다... )

<br>

<br>

## .mp4를 원하는 format_name을 가진 .mp3로 변환하기
---
이제 다운받은 .mp4 파일을 .mp3로 변환해야한다.

`moviepy` 라이브러리를 사용해서 mp3파일로 변환했다.

```
from moviepy.editor import * # pip install moviepy

default_filename = videos[0].default_filename # 기존 mp4 파일이름

# id format 변환 후 id_format.mp3 파일로 저장

id = int(all_values[row][0]) # 엑셀파일의 id column값

if int(id / 10) == 0 :
    id_format = "00" + str(id) # 한자리 수 id 일 경우 00x로 변환
elif int(id / 100) == 0 :
    id_format = "0" + str(id) # 두자리 수 id 일 경우 0xx로 변환
else:
    id_format = str(id) # 세자리 수 id 일 경우 id 값 그대로

new_filename = id_format +".mp3" # mp3로 변환할 파일 이름

video = VideoFileClip(os.path.join(parent_dir,default_filename)) # mp3로 변환할 기존 mp4파일 경로

video.audio.write_audiofile(os.path.join(parent_dir,new_filename)) # mp3로 변환 후 저장할 경로
```
<br>

나는 데이터셋 mp3 파일의 이름 포맷을 **title의 id 값**으로 하기로 정했기 때문에 이를 자동화하는 코드도 작성해주었다.

예를 들어 `title_1` 동영상 mp3의 경우 `001.mp3`로, `title_15`의 경우 `015.mp3`로 저장하기로 했다.

<br>

<br>

## 기존 .mp4 파일 지우기
---
이제 거의 다 했다..

현재 파일들이 저장된 디렉토리를 보면 `.mp4`와 `.mp3`파일이 같이 저장되어 있다.

나는 데이터셋으로 mp3 파일만 필요하기 때문에 mp4파일을 지우는 것까지 자동화 하고 싶었다.

코드는 아래와 같다!

<br>

```
import os

video.close() # process를 끝내야 mp4파일을 지울 수 있다.

if os.path.isfile(parent_dir + "/" + default_filename):
    os.remove(parent_dir + "/" + default_filename) # 기존 mp4 파일 지우기
```

<br>

해당 mp4파일이 존재하면 `os.remove()`로 그 파일을 지워주었다. 이때 `video.close()`를 안해주면 process가 끝나지 않아서 파일을 지우지 못한다는 ERROR가 난다.

<br>

<br>

## 일정 시간이 넘어가는 동영상은 제외하기
---
다 끝난 줄 알았는데...좀 더 보완할 곳이 생겼다.

내가 받아야 할 음성파일은 약 3분~7분 사이의 동영상이었다. 

하지만 현재는 무조건 검색결과의 첫 번째 동영상만 다운받으므로 동영상 재생 시간까지 체크해주는 과정이 필요했다.

만약 7분을 넘어가는 running time을 가지고 있다면 7분이 넘지 않는 그 다음 순서의 동영상을 대신 가져와야했다.

따라서 아래의 코드를 추가!

```
# 재생 시간이 7분 미만인 동영상을 찾아서 가져온다.
video_index=0
while True:
    running_time = soup.find_all(class_='video-time')[video_index].text # 동영상 재생시간 예시) 3:02
    splitted_time = running_time.split(":")

    if len(splitted_time) == 2: # 재생시간이 분, 초
        if int(splitted_time[0]) >= 7: # 재생시간이 7분 이상이면 다음 순서의 동영상을 가져온다.
            video_index += 1
            continue
        else :
            watch_url = soup.find_all(class_='yt-uix-sessionlink spf-link')[video_index]['href']
            break

    elif len(splitted_time) == 1 or len(splitted_time) == 3: # 재생시간이 초 또는 시,분,초
        video_index += 1 # 다음 순서의 동영상을 가져온다
        continue

```

<br>

검색결과의 첫 동영상부터 `video-time`을 **크롤링**해와서 7분이 넘어가는지 안넘어가는지 확인하고, 7분 미만의 동영상을 찾을 때 까지 **무한루프**를 돌렸다.

<br>

<br>

## 이제 진짜 끝!!
---
1. **엑셀 파일에 적어놓은 수 백개의 동영상 title 읽어오기**
2. **가져온 title에 해당하는 유튜브 동영상을 mp4파일로  다운받기**
3. **다운받은 mp4 파일을 원하는 format_name을 가진 mp3 파일로 변환하기**
4. **mp4 파일 지우기**

+**일정시간 미만의 동영상만 골라서 다운받기**

원래는 내가 일일이 수동으로 해야했을 위의 Task들을 모두 완벽(?)하게 **`자동화`** 하는 코드를 작성했고,

나와 똑같은 과제를 부여받았던 팀원들에게 재빠르게 뿌렸다!

팀원들이 고마워하는 걸 보고 아주아주 뿌듯했다 ㅎㅎ

팀원들의 시간을 세이브 해주었다는 생각에 팀에 기여했다는 뿌듯함이 뿜뿜! 기분 최고다!

나중에 또 이런 기회가 있다면 남들보다 조금 더 생각해보고 고민해봐서 팀원들의 번거로움을 줄여주는데 기여하고 싶다.


<br>

<br>

## 전체 코드
---
<script src="https://gist.github.com/kwonsye/c4c5985cfc63a972f951f148d0ba9461.js"></script>

<br>

<br>