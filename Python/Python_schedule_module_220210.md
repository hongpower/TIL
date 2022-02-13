# Schedule

### 지정한 시간마다 원하는 명령어 띄우기

```python
from datetime import datetime
import schedule
import time

# 현재 시간을 출력하는 job 함수 생성
def job():
    now = datetime.now()
    print(now)

# 매 minute마다 job이라는 함수 실행
schedule.every().minutes.do(job)

# 매 hour마다
schedule.every().hour.do(job)

# 매 날마다 정해진 시간에
schedule.every().day.at('17:42').do(job)

# 매 요일마다
schedule.every().monday.do(job)

while True:
    schedule.run_pending()
    time.sleep(1)
```

