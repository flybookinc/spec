# 서비스 연계 API 명세서

## 서비스 개요
본 문서는 플라이북 AI 서비스와 연계시 필요한 API 목록입니다.

## 기본 정보(예시)
- **Base URL**: `https://api.example.com`
- **회원인증 방식**: Bearer Token (JWT)
- **서버인증 방식**: API_KEY
- **Content-Type**: `application/json`
- **응답 형식**: JSON

## 인증 흐름
1. 사용자는 `/login` 엔드포인트를 통해 로그인하여 JWT를 발급받음.
2. 발급받은 JWT는 `Authorization` 헤더에 `Bearer <token>` 형식으로 포함되어야 함.
3. JWT는 1시간 동안 유효하며, 만료 시 재로그인 필요.

### 공통 인증 방식

- **회원 인증**: JWT (JSON Web Token)

  - 요청 Header: `Authorization: Bearer {JWT_TOKEN}`

- **서버 인증**: API Key

  - 요청 Header: `X-API-KEY: {API_KEY}`

---

### 1. 로그인

- **URL**: `/api/login`
- **Method**: `POST`
- **Headers**:

  - `X-API-KEY: {API_KEY}`

- **Request Body** (JSON):

```json
{
  "id": "user01",
  "password": "password123"
}
```

- **Response**:

  - **성공 (200 OK)**:

  ```json
  {
    "token": "eyJ0eXAiOiJKV1QiLCJhbGciOi...",
    "user": {
      "id": 1,
      "name": "홍길동"
    }
  }
  ```

  - **실패**:
    - **400 Bad Request**: 잘못된 요청 형식.
      ```json
      {
        "status": "error",
        "message": "Invalid id or password format"
      }
      ```
    - **401 Unauthorized**: 잘못된 자격 증명.
      ```json
      {
        "status": "error",
        "message": "Invalid id or password"
      }
      ```

---

### 2. 도서 조회

- **URL**: `/api/books`
- **Method**: `GET`
- **Headers**:

  - `Authorization: Bearer {JWT_TOKEN}`
  - `X-API-KEY: {API_KEY}`

- **Query Parameters**:
  - `isbn` (optional): ISBN 번호

- **Response**:
  - **성공 (200 OK)**:
  ```json
  {
    "book_id": 123,
    "title": "채식주의자",
    "isbn": "9788936480615",
    "loan_state": "대출가능"
  }
  ```

---

### 3. 대출이력 조회

- **URL**: `/api/borrow/history`
- **Method**: `GET`
- **Headers**:

  - `Authorization: Bearer {JWT_TOKEN}`
  - `X-API-KEY: {API_KEY}`

- **Response**:
  - **성공 (200 OK)**:

```json
[
  {
    "borrow_id": 1001,
    "book_id": 124,
    "book_title": "채식주의자",
    "book_isbn": "9788936480615",
    "borrow_date": "2025-04-01"
  },
  {
    "borrow_id": 1002,
    "book_id": 124,
    "book_title": "채식주의자",
    "book_isbn": "9788936480615",
    "borrow_date": "2025-05-01"
  }
]
```
