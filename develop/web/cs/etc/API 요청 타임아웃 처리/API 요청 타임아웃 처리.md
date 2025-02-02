# API 요청 타임아웃 처리

- API 요청 타임아웃은 클라이언트가 서버로 요청을 보낸 후 응답을 기다리는 최대 시간을 설정하는 방식.
- 서버가 요청을 처리하지 못하거나 응답이 지연될 경우 클라이언트는 타임아웃을 트리거하여 대기 상태를 종료.
- 타임아웃 처리는 시스템 안정성과 사용자 경험을 개선하고, 리소스 낭비를 방지하기 위해 필수적.

---

# 타임아웃 처리의 중요성

## **시스템 안정성**

- 무한 대기를 방지해 리소스를 적절히 해제.

## **사용자 경험**

- 오래 걸리는 요청을 중단하고 에러 메시지를 제공하여 사용자의 불편 최소화.

## **리소스 보호**

- 서버 또는 네트워크의 부하가 과도해지는 것을 방지.

---

# 클라이언트 측 타임아웃 처리

## **HTTP 클라이언트에서 타임아웃 설정**

### **Node.js (Axios)**

```jsx
const axios = require('axios');

axios.get('https://api.example.com/data', { timeout: 5000 }) // 5초 타임아웃
  .then(response => {
    console.log(response.data);
  })
  .catch(error => {
    if (error.code === 'ECONNABORTED') {
      console.error('Request timed out');
    } else {
      console.error(error.message);
    }
  });
```

### **Python (Requests)**

```python
import requests

try:
    response = requests.get('https://api.example.com/data', timeout=5) # 5초 타임아웃
    print(response.json())
except requests.exceptions.Timeout:
    print("Request timed out")
except requests.exceptions.RequestException as e:
    print(f"An error occurred: {e}")
```

### **JavaScript (Fetch API)**

```jsx
const controller = new AbortController();
const timeout = setTimeout(() => controller.abort(), 5000); // 5초 후 요청 취소

fetch('https://api.example.com/data', { signal: controller.signal })
  .then(response => response.json())
  .then(data => console.log(data))
  .catch(error => {
    if (error.name === 'AbortError') {
      console.error('Request timed out');
    } else {
      console.error(error.message);
    }
  })
  .finally(() => clearTimeout(timeout));
```

---

# 서버 측 타임아웃 처리

## **HTTP 서버에서 요청 타임아웃 설정**

### **Node.js (Express)**

```jsx
javascript
코드 복사
const express = require('express');
const app = express();

// 응답 타임아웃 설정 (10초)
app.use((req, res, next) => {
    res.setTimeout(10000, () => {
        console.error('Request timed out');
        res.status(408).send('Request Timeout');
    });
    next();
});

app.get('/data', (req, res) => {
    // 일부러 지연시키는 예제
    setTimeout(() => res.send('Response after delay'), 5000);
});

app.listen(3000, () => console.log('Server running on port 3000'));
```

### **Python (Flask)**

- Flask 자체적으로 타임아웃 설정은 없으므로, WSGI 서버(예: Gunicorn)에서 설정.

```bash
gunicorn -w 4 -b 127.0.0.1:5000 --timeout 30 app:app
```

## **데이터베이스 타임아웃 설정**

- API가 데이터베이스와 연결될 경우, 데이터베이스 쿼리에도 타임아웃을 설정.

### **MySQL 예시**

```sql
SET SESSION MAX_EXECUTION_TIME=5000; -- 쿼리 최대 실행 시간 5초
```

### **MongoDB (Node.js)**

```jsx
const mongoose = require('mongoose');
mongoose.connect('mongodb://localhost:27017/mydb', {
    socketTimeoutMS: 5000, // 5초 타임아웃
});
```

---

# 타임아웃 처리 시 고려 사항

## **적절한 타임아웃 값 설정**

- 요청의 복잡성과 네트워크 환경에 따라 타임아웃 값을 조정.
- 너무 짧으면 정상적인 요청이 실패할 수 있고, 너무 길면 사용자 경험이 저하.

## **재시도 로직 구현**

- 요청이 실패하면 일정 횟수 재시도하도록 구현.
- **Exponential Backoff** 방식 사용으로 서버 부하 방지.

## **Graceful Fallback**

- 타임아웃 발생 시 대체 데이터나 기본 메시지를 제공하여 사용자의 혼란 최소화.

```jsx
console.error('Request timed out, showing cached data');
displayCachedData();
```

## **로깅 및 모니터링**

- 타임아웃 발생 시 로그를 기록해 서버의 문제점을 분석하고 대응.

## **네트워크 안정성 고려**

- 타임아웃 처리는 네트워크 환경의 영향을 받을 수 있으므로 안정적인 연결을 위한 전략 필요.

---

# 장점

- 리소스 낭비 방지 및 시스템 안정성 확보.
- 네트워크 및 서버 문제 시 빠른 복구 가능.
- 사용자 경험 향상 및 효율적인 트래픽 관리.

# 단점

- 재시도 설정 시 과도한 요청이 서버에 부하를 줄 수 있음.
- 너무 짧은 타임아웃은 정상적인 요청을 차단할 수 있음.

---

# **요약**

- API 요청 타임아웃 처리는 클라이언트와 서버 모두에서 적절히 설정하여 리소스를 보호하고 사용자 경험을 개선하는 중요한 요소.
- 타임아웃 값을 합리적으로 설정하고, 재시도 로직, 로깅, Fallback 메커니즘 등을 함께 구현하여 효율적이고 안정적인 API를 설계해야 함.