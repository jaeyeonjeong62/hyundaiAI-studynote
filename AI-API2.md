## API란 무엇인가 — REST API의 기본 개념

**API(Application Programming Interface)**

서로 다른 소프트웨어가 정해진 규칙으로 대화하는 창구

**REST API(Representational State Transfer API)**

- 웹에서 가장 널리 쓰이는 API 설계 방식
- HTTP 프로토콜을 사용해 요청을 보내고 응답을 받음
- 데이터는 주로 **JSON(JavaScript Object Notation)** 형식으로 주고받음

**REST API의 핵심 요소**

|요소|설명|예시|
|---|---|---|
|엔드포인트(Endpoint)|API 서버의 주소|`https://api.openai.com/v1/chat/completions`|
|HTTP 메서드|요청의 종류|GET(조회), POST(생성), PUT(수정), DELETE(삭제)|
|헤더(Header)|요청 부가 정보|인증 키, 콘텐츠 타입 등|
|바디(Body)|요청 본문 데이터|JSON 형식의 메시지, 모델명 등|
|응답(Response)|서버가 돌려주는 결과|JSON 형식의 AI 답변|

AI 텍스트 생성 API를 호출할 때는 보통 `POST` 메서드를 사용하고, 헤더에 **API 키**를 담아 자신이 정당한 사용자임을 증명한다.


## 환경변수와 .env 파일로 Key 안전하게 관리하기

### .env 파일 방식

- **.env 파일**은 환경변수를 파일로 관리하는 표준적인 방법
- 프로젝트 루트 폴더에 `.env` 파일을 만들고 키를 저장함

```
OPENAI_API_KEY=sk-proj-xxxxxxxxxxxxxxxxxxxx
GOOGLE_API_KEY=AIzaxxxxxxxxxxxxxxxxxxxxxxxx
ANTHROPIC_API_KEY=sk-ant-xxxxxxxxxxxxxxxxxxxx
```

```
from dotenv import load_dotenv
import os

# .env 파일의 환경변수를 불러옵니다
load_dotenv()

openai_key = os.getenv("OPENAI_API_KEY")
google_key = os.getenv("GOOGLE_API_KEY")
anthropic_key = os.getenv("ANTHROPIC_API_KEY")

print("OpenAI 키:", "설정됨" if openai_key else "미설정")
print("Google 키:", "설정됨" if google_key else "미설정")
print("Anthropic 키:", "설정됨" if anthropic_key else "미설정")
```

### .gitignore 설정

- `.env` 파일은 반드시 `.gitignore`에 추가하여 깃 저장소에 올라가지 않도록 함
- 프로젝트 폴더에 `.gitignore` 파일을 만들고 다음 내용을 추가함

```
# API 키 파일 제외
.env
.env.local
.env.*.local
```

---

## 외부 API 연동 핵심 정리

### 1. 함수·메서드·HTTP 메서드는 서로 다르다

| 표현 | 무엇인가? | 역할 |
|---|---|---|
| HTTP `GET` | HTTP 요청 방식 | 서버에 데이터 조회 요청 |
| `requests.get()` | `requests` 모듈의 파이썬 함수 | HTTP GET 요청을 보냄 |
| HTTP `POST` | HTTP 요청 방식 | 서버에 데이터를 보내 처리 요청 |
| `requests.post()` | `requests` 모듈의 파이썬 함수 | HTTP POST 요청을 보냄 |
| `data.get("name")` | 딕셔너리의 메서드 | 이미 받은 데이터에서 값을 찾음 |

**함수**는 호출해서 쓰는 코드이고, **메서드**는 객체에 속한 함수


### 2. API 요청에서 자주 쓰는 것

| 코드 | 의미 |
|---|---|
| `load_dotenv()` | `.env` 파일의 값을 환경변수로 불러옴 |
| `os.environ["API_KEY"]` | 환경변수에서 키를 읽음. 키가 없으면 오류가 나서 설정 누락을 알 수 있음 |
| `requests.get(url, params=..., timeout=...)` | 조회 조건과 시간 제한을 정해 GET 요청 |
| `requests.post(url, json=..., headers=..., timeout=...)` | JSON 데이터와 인증 정보를 담아 POST 요청 |
| `response.raise_for_status()` | 401·404·429 같은 HTTP 오류 응답이면 예외 발생 |
| `response.json()` | 서버의 JSON 응답을 파이썬 자료형으로 변환 |
| `data.get("key", 기본값)` | 딕셔너리에 키가 없어도 기본값을 반환 |


### 3. 전체 예시: 날씨 조회 → AI 설명 요청

`.env` 파일:

```dotenv
OPENWEATHER_API_KEY=발급받은_날씨_API_키
OPENAI_API_KEY=발급받은_OpenAI_API_키
```

설치:

```bash
pip install requests python-dotenv
```

파이썬 코드:

```python
import os

import requests
from dotenv import load_dotenv

load_dotenv()

weather_key = os.environ["OPENWEATHER_API_KEY"]
openai_key = os.environ["OPENAI_API_KEY"]

try:
    # requests.get() 함수로 HTTP GET 요청을 보낸다.
    weather_response = requests.get(
        "https://api.openweathermap.org/data/2.5/weather",
        params={
            "q": "Seoul",
            "appid": weather_key,
            "units": "metric",
            "lang": "kr",
        },
        timeout=15,
    )
    weather_response.raise_for_status()
    weather_data = weather_response.json()

    temperature = weather_data["main"]["temp"]

    # 딕셔너리의 .get() 메서드: HTTP 요청과 관계없다.
    weather_list = weather_data.get("weather", [])
    description = (
        weather_list[0].get("description", "정보 없음")
        if weather_list else "정보 없음"
    )

    # requests.post() 함수로 HTTP POST 요청을 보낸다.
    ai_response = requests.post(
        "https://api.openai.com/v1/chat/completions",
        headers={"Authorization": f"Bearer {openai_key}"},
        json={
            "model": "gpt-4.1-mini",
            "messages": [
                {
                    "role": "user",
                    "content": (
                        f"서울은 현재 {temperature}°C이고 "
                        f"날씨는 {description}이야. 한 문장으로 설명해줘."
                    ),
                }
            ],
        },
        timeout=30,
    )
    ai_response.raise_for_status()
    ai_data = ai_response.json()

    print(ai_data["choices"][0]["message"]["content"])

except requests.exceptions.Timeout:
    print("서버 응답 시간이 초과됐어요.")
except requests.exceptions.HTTPError as error:
    print(f"HTTP 오류: {error.response.status_code}")
except requests.exceptions.RequestException as error:
    print(f"네트워크 요청 오류: {error}")
```

**날씨 서버에 HTTP GET → JSON에서 기온 추출 → AI 서버에 HTTP POST → 생성된 답변 출력**

