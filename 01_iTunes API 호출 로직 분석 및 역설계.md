## 요약
- 내용: AI(Gemini)와 협업하여 작성된 파이썬 코드를 바탕으로, API 호출(Request) 및 데이터 파싱(Parsing) 로직을 역설계하며 학습함.
- 목표: iTunes Open API를 호출하여 특정 아티스트의 상위 3곡 정보를 파싱하는 로직 이해.
- 비즈니스 활용: 고객 선호 아티스트 기반의 '웰컴 뮤직 추천 API 연동 기획' 등에 응용 가능.

## 도구 및 환경 (VS Code)
: 파이썬 언어를 적을 수 있게 도와주는 도구

- 코드 편집기 (Edit Area) : 기획의 로직을 파이썬 코드로 구현하는 공간 (~를 불러와)
- 터미널 (Terminal) : 컴퓨터에게 명력을 내릴 때(Input), 코드의 실행 결과를 확인할 때(Output)
- Problems : 코드에 오타나 문법이 틀렸을 때 경고를 주는 모니터링 기능.
- Output : 로그 기록
- Agent : Github Copilot 같은 AI 기능. 구현하고 싶은 기능을 말하면 코드를 제안해 주는 AI 협업 도구.

## API 호출 및 데이터 파싱 과정
```python
import requests
def search_music(artist_name):
    url = f"https://itunes.apple.com/search?term={artist_name}&entity=song&limit=3"
    
    response = requests.get(url)
    
    if response.status_code == 200:
        data = response.json()
        results = data.get('results', [])
        
        print(f"\n[호텔 시스템] 고객님 맞춤의 '{artist_name}' 데이터를 불러왔습니다.")
        print("="*50)
        
        for song in results:
            track_name = song.get('trackName')     
            album_name = song.get('collectionName')
            image_url = song.get('artworkUrl100') 
            
            print(f"곡명: {track_name}")
            print(f"앨범: {album_name}")
            print(f"이미지: {image_url}")
            print("-" * 50)
    else:
        print("API 통신 에러: 데이터를 불러올 수 없습니다.")
search_music("xdinary heroes")
```

## 용어 정리
- **파라미터 (Parameter)**:
  - 매개변수. 전달되는 조건값. 파라미터가 바뀌면 결과도 바뀜.
  - '이 조건'을 가지고 일을 처리하라는 가이드라인.
  - 예: 애플 서버(API)에게 검색어(`term`), 종류(`entity`), 개수(`limit`)라는 세 조건을 줄 테니 그에 맞춰 데이터를 가져와.
- **파싱 (Parsing)**:
  - 받아온 데이터 뭉치를 우리가 원하는 정보(곡명, 이미지 등)로 쪼개서 정리하는 과정.
  - 거대한 데이터 중 기획에 필요한 정보만 골라내어 사용하기 좋은 형태로 가공하는 단계.
- **`.`(Dot)**:
  - 소유권과 실행 명령.
  - `~의 안에 있는` (소유와 위치) 혹은 `~해라` (기능 실행)를 뜻함. 데이터 구조 안으로 깊게 파고들거나 특정 동작을 수행할 때 사용
- **상태 코드(200)**: API 통신이 성공적으로 이루어졌음을 뜻하는 약속된 숫자.

## API 로직 Study
- **`pip3 install requests`**: 배달 앱 설치와 같음. 데이터를 가져오는 데 필요한 'requests'라는 부품을 내 컴퓨터에 설치하는 과정.
- **`import requests`**: 설치한 'requests' 부품을 상자에서 꺼내 현재 코드 파일로 불러오기.
- **`def search_music(artist_name):`**: '음악 검색'이라는 기능을 정의(Define). 이때 '아티스트 이름'이라는 데이터가 반드시 필요하다고 설정함.
- **`url = f"..."`**: 문자열 안에 변수를 쏙 집어넣겠다는 선언. `{artist_name}` 자리에 실제 검색어가 들어감.
- **`https://itunes.apple.com/search?`**: 애플 서버에 "데이터 찾으러 왔습니다"라고 벨 누르는 입구 주소.
    * `term={artist_name}`: DB에서 이 아티스트를 찾으세요.
    * `entity=song`: 앨범/뮤비 말고 '노래' 정보만 보내주세요.
    * `limit=3`: 너무 많으면 힘드니 딱 3개만 보여주세요.
- **`response = requests.get(url)`**: requests(행동)가 url 주소로 신호를 보내서 서버로부터 돌아온 모든 정보(**response, 결과물**)를 담아오기.
- **`if response.status_code == 200:`**: 배달이 성공했는지 확인. 응답 코드가 200이면 "데이터가 정상 도착했다"는 약속된 신호.
- **`data = response.json()`**: 서버에서 받은 텍스트 뭉치(JSON)를 파이썬이 요리하기 편한 사전(Dictionary) 구조로 변환.
- **`results = data.get('results', [])`**: 데이터 뭉치에서 'results'라는 이름표가 붙은 노래 목록만 쏙 뽑아내기. 없으면 빈 바구니(`[]`).
- **`for song in results:`**: 반복 추출. 리스트 안에 있는 곡들을 하나하나 꺼내서 'song'이라는 임시 바구니에 담아 확인 (총 3번 반복).
- **`track_name = song.get('trackName')`**: Key값 접근. 데이터가 '이름:값' 쌍으로 되어 있는데, `trackName`이라는 이름표를 열어 실제 '곡 제목'을 가져와 저장함.
- **`else: print("API 통신 에러")`**: 데이터가 안 왔을 때 사용자에게 띄워줄 예외 문구.
- **`search_music("Xdinary Heroes")`**: "엑스디너리 히어로즈"라는 구체적인 데이터를 던져주며 위에서 만든 함수를 실제로 실행
