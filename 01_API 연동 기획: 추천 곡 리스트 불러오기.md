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
* **파라미터 (Parameter)**: **전달되는 조건값.** * '이 조건'을 가지고 일을 처리하라는 가이드라인입니다. 아티스트 이름이라는 조건을 받아서 처리하는 매개변수입니다.
    * 애플 서버(API)에게 검색어(`term`), 종류(`entity`), 개수(`limit`)라는 세 조건을 줄 테니 그에 맞춰 데이터를 가져오라고 명령하는 기준이 됩니다. 파라미터가 바뀌면 결과도 바뀝니다.
* **파싱 (Parsing)**: **데이터를 쪼개서 정리하는 과정.** * 받아온 데이터 뭉치를 우리가 원하는 정보(곡명, 이미지 등)로 쪼개서 정리하는 과정입니다. 거대한 데이터 중 기획에 필요한 정보만 골라내어 사용하기 좋은 형태로 가공하는 단계입니다.
* **. (Dot)**: **소유권과 실행 명령.** * `~의 안에 있는` (소유와 위치) 혹은 `~해라` (기능 실행)를 뜻합니다. 데이터 구조 안으로 깊게 파고들거나 특정 동작을 수행할 때 사용합니다.
* **JSON 구조**: **'이름:값(Key:Value)'의 쌍으로 이루어진 데이터 형식.** * 딕셔너리 구조로 되어 있으며, 기획자는 이 수많은 이름:값 쌍 중에서 서비스 화면에 뿌려줄 어떤 'Key'가 필요한지 정확히 정의하는 것이 중요합니다.


- **상태 코드(200)**: API 통신이 성공적으로 이루어졌음을 뜻하는 약속된 숫자.
