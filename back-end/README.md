# Back-end

- [Directory Structure](#directory-structure)
- [Installation](#installation)
- [Migration](#migration)
- [Endpoints](#endpoints)
  - [Plant](#plant)
    - [`GET` /plants](#get-plants)
  - [UserPlant](#userplant)
    - [`POST` /user-plants](#post-user-plants)
    - [`GET` /user-plants](#get-user-plants)
    - [`GET` /user-plants/:id](#get-user-plantsid)
  - [Auth](#auth)
    - [`POST` /auth](#post-auth)
    - [`GET` /auth](#get-auth)

## Directory Structure

```shell
.
├── config
├── src
│   ├── entity        # Models
│   ├── migration     # Database migrations
│   └── index.ts      # Entry point
├── tests             # Unit tests
├── .env.example      # Environment variable example for production
└── ormconfig.json    # Default TypeORM configuration
```

## Installation

```shell
npm install
```

## Migration

```shell
# Run migration
npm run typeorm migration:run

# Revert migration
npm run typeorm migration:revert
```

## Endpoints

Description에 Optional 키워드가 없다면 Required를 전제로 함.

### Plant

#### `GET` /plants

> Query Parameters

| Key  | Value    | Description                                  |
| ---- | -------- | -------------------------------------------- |
| name | `string` | _Optional_ 검색하려는 이름을 포함하는 문자열 |

> Responses

| Code | Return         |
| ---- | -------------- |
| 200  | `Array<Plant>` |

### UserPlant

#### `POST` /user-plants

> Body Paramerters

| Key   | Value    | Description                                                  |
| ----- | -------- | ------------------------------------------------------------ |
| user  | `User`   | 반려 식물을 등록할 유저의 기본키 (User의 id)                 |
| plant | `Plant`  | 반려 식물로 등록할 식물의 기본키 (Plant의 id)                |
| name  | `string` | _Optional_ 반려 식물의 이름 (생략시 연관 식물의 기본값)      |
| water | `string` | _Optional_ 반려 식물의 급수 주기 (생략시 연관 식물의 기본값) |

> Responses

| Code | Return      | Description           |
| ---- | ----------- | --------------------- |
| 201  | `UserPlant` | 생성된 UserPlant 객체 |

#### `GET` /user-plants

> Responses

| Code | Return             | Description    |
| ---- | ------------------ | -------------- |
| 200  | `Array<UserPlant>` | UserPlant 전체 |

#### `GET` /user-plants/:id

> Path Parameters

| Key | Value          | Description  |
| --- | -------------- | ------------ |
| id  | `string(uuid)` | UserPlant id |

> Responses

| Code | Return      | Description            |
| ---- | ----------- | ---------------------- |
| 200  | `UserPlant` | id값과 일치하는 데이터 |
| 404  | Not Found   | 일치하는 데이터 없음   |

### Auth

#### `POST` /auth

> Body Parameters

| Key      | Value    | Description   |
| -------- | -------- | ------------- |
| email    | `string` | User email    |
| password | `string` | User password |

> Responses

| Code | Return          | Description   |
| ---- | --------------- | ------------- |
| 200  | `string(token)` | JWT 토큰      |
| 401  | Unauthorized    | 비밀번호 오류 |

#### `GET` /auth

> Header Parameters

| Key           | Value    | Description |
| ------------- | -------- | ----------- |
| Authorization | `string` | Bearer 토큰 |

> Responses

| Code | Return | Description             |
| ---- | ------ | ----------------------- |
| 200  | `User` | 토큰을 발급한 유저 정보 |
