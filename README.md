# 🛡️ AI-Enhanced Wave Defense: MapleStory World
> **생성형 AI(Gemini)가 만드는 '살아있는 보스'와의 전략적 디펜스 게임**

<br>

## 📅 Project Background
**"왜 디펜스 게임은 항상 똑같은 패턴일까?"**
기존의 메이플스토리월드 내 웨이브 디펜스 게임들은 단순한 직업 선택과 반복적인 막기 형식에 그쳐, 플레이어의 능동적인 개입이 제한적이었습니다.
저희 팀은 **"매일 기분이 달라지고, 유저와 대화하며 패턴이 바뀌는 보스"**를 도입하여, 플레이어들이 매판 새로운 전략과 팀워크를 고민하게 만드는 게임을 개발했습니다.

<br>

## 📝 Project Summary
* **플랫폼:** MapleStory World (PC/Mobile)
* **장르:** 4인 멀티플레이 웨이브 디펜스
* **핵심 컨셉:**
    * **Generative AI Boss:** Google Gemini API를 활용하여 보스와의 채팅 상호작용 구현. 보스의 '오늘의 기분'과 대화 내용에 따라 스킬 패턴과 난이도가 실시간으로 변화합니다.
    * **Strategic Teamwork:** 직업-몬스터 간 **속성 상성 시스템**을 도입하여, 무지성 사냥이 아닌 전략적인 파티 조합과 포지셔닝을 요구합니다.

<br>

## 🛠 Tech Stack

<div align="left">
  <h3>Client (Game Logic)</h3>
  <img src="https://img.shields.io/badge/MapleStory%20World-FF4785?style=flat&logo=maplestory&logoColor=white"/>
  <img src="https://img.shields.io/badge/Lua-2C2D72?style=flat&logo=lua&logoColor=white"/>
  <br>
  <h3>AI Server & Backend</h3>
  <img src="https://img.shields.io/badge/Python-3776AB?style=flat&logo=python&logoColor=white"/>
  <img src="https://img.shields.io/badge/Google%20Gemini%20API-8E75B2?style=flat&logo=google&logoColor=white"/>
  <img src="https://img.shields.io/badge/Render-46E3B7?style=flat&logo=render&logoColor=white"/>
  <br>
  <h3>Tools</h3>
  <img src="https://img.shields.io/badge/Postman-FF6C37?style=flat&logo=postman&logoColor=white"/>
</div>

<br>

## 💻 Key Logic: Deterministic Daily Mood System
별도의 DB 연동 없이도, **모든 게임 채널(서버)에서 동일한 날짜에는 동일한 '보스의 기분(Mood)'이 적용**되도록 설계한 Python 백엔드 로직입니다.

### 1. KST 기준 날짜 Key 생성 (`get_kst_day_key`)
서버 위치(UTC)와 상관없이 한국 시간(KST) 기준으로 하루 단위를 정수(Integer)화하여 Key를 생성합니다.
```python
def get_kst_day_key(offset_days: int = 0) -> int:
    """KST(UTC+9) 기준으로 날짜 단위 key를 반환"""
    now_utc = datetime.datetime.utcnow()
    # KST 오프셋 적용 및 기준일(epoch)로부터 경과한 일수 계산
    kst = now_utc + datetime.timedelta(seconds=KST_OFFSET_SEC) + datetime.timedelta(days=offset_days)
    epoch = datetime.datetime(2025, 9, 1)
    days = int((kst - epoch).total_seconds() // 86400)
    return days
