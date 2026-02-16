- 요약 : AI(Gemini)와 협업하여 작성된 파이썬 코드를 바탕으로, API 호출(Request) 및 데이터 파싱(Parsing) 로직을 역설계하며 학습함.
- **목표**: iTunes Open API를 호출하여 특정 아티스트의 상위 3곡 정보를 파싱(Parsing)하는 로직 이해
- **비즈니스 활용**: 고객 선호 아티스트 기반의 웰컴 뮤직 추천 API 연동 기획 등에 응용 가능

## 실습 코드 분석
: AI와 함께 설계한 iTunes Search API 활용 데이터 수집 로직

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
search_music("xdinary heroes")```

## 기획자 관점에서 본 VS Code 주요 기능
- **Edit Area**: 기획 로직을 파이썬 코드로 구현하는 공간.
- **Terminal**: 컴퓨터에게 명령을 내리고(Input), 실행 결과(Output)를 확인하는 창.
- **GitHub Copilot (Agent)**: 구현하고 싶은 기능을 설명하면 AI가 코드를 제안하는 협업 도구.

## API 호출 및 데이터 파싱 과정

### 1. 환경 설정 및 부품 불러오기
- `pip3 install requests`: HTTP 요청을 보내기 위한 외부 라이브러리(부품) 설치.
- `import requests`: 설치한 부품을 현재 코드 파일로 불러옴.

### 2. 함수 정의 및 요청 (Request)
- `def search_music(artist_name):`: 아티스트 이름을 조건값(Parameter)으로 받는 함수 정의.
- `url = f"..."`: 애플 서버 주소에 변수를 담아 요청 보낼 주소 생성.
    - `term`: 검색어 (아티스트 명)
    - `entity=song`: 데이터 종류 (곡 정보만)
    - `limit=3`: 가져올 개수 제한

### 3. 응답 처리 및 데이터 변환 (Response & Parsing)
- `response = requests.get(url)`: 서버로부터 돌아온 모든 정보를 변수에 담음.
- `data = response.json()`: 서버의 텍스트 응답을 파이썬이 다룰 수 있는 **Dictionary(Key:Value)** 구조로 변환.

### 4. 반복문(For)을 통한 정보 추출
- `for song in results:`: 받아온 3개의 곡 정보 뭉치에서 하나씩 꺼내어 처리.
- `song.get('trackName')`: 데이터 쌍 중에서 'trackName'이라는 **Key**를 찾아 실제 곡 제목(**Value**)을 가져옴.

---

## 💡 주요 개념 정리
- **파라미터(Parameter)**: 함수에 전달하는 조건값. 이 값이 바뀌면 결과도 바뀜 (예: 아티스트명).
- **파싱(Parsing)**: 받아온 거대한 데이터 뭉치를 기획에 필요한 정보(곡명, 이미지 등)로 쪼개고 정리하는 과정.
- **상태 코드(200)**: API 통신이 성공적으로 이루어졌음을 뜻하는 약속된 숫자.
