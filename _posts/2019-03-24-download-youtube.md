---
layout: post
comments: true
title: "python으로 노가다 자동화하기_수백개의 Youtube 영상을 mp3파일로 저장하는 방법"
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

일단 첫 번째 문제부터 해결해보자.

`python으로 엑셀파일 읽기` 검색 후 **`openpyxl`** 라는 라이브러리가 있다는 것을 알게되었다.

<a href="">블로그</a>도 참고해서 엑셀 파일에 저장된 데이터들을 읽어왔다.

현재 내 엑셀 파일 `데이터셋_soft.xlsx` 에는 아래와 같은 형식으로 **2개**의 col과 header포함 **151개**의 row들이 저장되어있다.

| id | title   |
|----|---------|
| 1  | title_1 |
| 2  | title_2 |
| 3  | title_3 |

```
from openpyxl import load_workbook # pip install openpyxl

# 해당 경로에서 엑셀 파일 가져오기
load_wb = load_workbook("C:/Users/kwons/Desktop/데이터셋_soft.xlsx", data_only=True)
# 시트 이름으로 불러오기
load_ws = load_wb['Sheet1']

print('-----헤더를 제외한 엑셀의 모든 행과 열 저장-----')
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

헤더를 제외하기 위해서 셀 `A2`(=1) 부터 `B151`(=title_150)까지 `all_values`에 저장했다.

`all_values`에는 `[[1, title_1], [2, title_2], [3, title_3], ... ,[150, title_150]]` 이렇게 이차원 배열이 저장된다.

*참고 >> `A`와 `B`는 각각 1열, 2열이고 알파벳 뒤에 붙은 숫자는 행이다. 예를 들어 `A1`의 경우 1행1열인 `id`이며 `B3`의 경우 3행2열인 `title_2` 이다.

