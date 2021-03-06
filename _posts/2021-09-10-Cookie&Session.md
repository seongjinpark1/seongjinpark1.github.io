---
title: 'Cookie & Session'
tags: [TIL, JavaScript, Authentication]
style: fill
color: danger
description: '쿠키는 서버에서 클라이언트에 데이터를 저장하는 방법의 하나입니다.
쿠키를 이용하는 것은 단순히 서버에서 클라이언트에 쿠키를 전송하는 것만 의미하지 않고 클라이언트에서 서버로 쿠키를 전송하는 것도 포함됩니다.'
---

# 왜 cookie와 session을 사용하는가?

HTTP 프로토콜의 특징이자 약점을 보완하기 위해서 사용됩니다.

## HTTP특징

**1. Connectionless(비 연결지향)**: 클라이언트에서 서버에 요청을 보내면 서버는 클라이언트에 응답을 하고 접속을 끊는 특성이 있습니다.

**2. Stateless(무상태성)**: HTTP통신은 요청을 응답하고 접속을 끊기 때문에 클라이언트의 상태정보를 알 수 없습니다. 이를 Stateless하다고 합니다.

> 하지만 실제로는 데이터 유지가 필요한 경우가 많습니다.
> 정보가 유지되지 않으면, 매번 페이지를 이동할 때마다 로그인을 다시 하거나, 상품을 선택했는데 구매 페이지에서 선택한 상품의 정보가 없거나 하는 등의 일이 발생할 수 있습니다.
> **따라서, Stateful경우를 대처하기 위해서 쿠키와 세션을 사용합니다.**

# Cookie

## Cookie란?

쿠키는 서버에서 클라이언트에 데이터를 저장하는 방법의 하나입니다.
쿠키를 이용하는 것은 단순히 서버에서 클라이언트에 쿠키를 전송하는 것만 의미하지 않고 클라이언트에서 서버로 쿠키를 전송하는 것도 포함됩니다.

## Cookie 특징

서버는 쿠키를 이용하여 데이터를 저장하고 원할 때 이 데이터를 다시 불러와 사용할 수 있습니다.
하지만 데이터를 저장한 이후 아무 때나 데이터를 가져올 수 없습니다. 데이터를 저장한 이후 특정 조건들이 만족하는 경우에만 다시 가져올 수 있습니다.
이런 조건들은 **쿠키 옵션**으로 표현할 수 있습니다.

#### 1. Domain

도메인은 `www.google.com`과 같은 서버에 접속할 수 있는 이름입니다.
쿠키 옵션에서 도메인은 포트 및 서브 도메인 정보, 세부 경로를 포함하지 않습니다.
따라서 요청해야 할 URL이 `http://www.localhost.com:3000/users/login`이라 하면 여기서 Domain은 `localhost.com`이 됩니다.
**만약 쿠키 옵션에서 도메인 정보가 존재한다면 클라이언트에서는 쿠키의 도메인 옵션과 서버의 도메인이 일치해야만 쿠키를 전송할 수 있습니다.**

#### 2. Path

세부 경로는 서버가 라우팅할 때 사용하는 경로입니다.
만약 요청해야 하는 URL이 `http://www.localhost.com:3000/users/login`인 경우라면 여기에서 Path, 세부 경로는 `/user/login`이 됩니다.
명시하지 않으면 기본 값으로는 `/`으로 설정되어 있습니다.
Path 옵션의 특징은 설정된 path를 전부 만족하는 경우 요청하는 Path가 추가로 더 존재하더라도 쿠키를 서버에 전송할 수 있습니다.
**즉 Path가 `/users`로 설정되어 있고, 요청하는 세부 경로가 `/users/login`인 경우라면 쿠키 전송이 가능합니다.**

#### MaxAge or Expires

쿠키가 유효한 기간을 정하는 옵션입니다.
MaxAge는 앞으로 몇 초 동안 쿠키가 유효한지 설정하는 옵션입니다.
Expires은 MaxAge와 비슷합니다. 다만 언제까지 유효한지 `Date`를 지정합니다. 이때 클라이언트의 시간을 기준으로 합니다.
**이후 지정된 시간, 날짜를 초과하게 되면 쿠키는 자동으로 파괴됩니다.
하지만 두 옵션이 모두 지정되지 않는 경우에는 브라우저의 탭을 닫아야만 쿠키가 제거될 수 있습니다.**

#### Secure

쿠키를 전송해야 할 떄 사용하는 프로토콜에 따른 쿠키 전송 여부를 결정합니다.
**만약 해당 옵션이 `true`로 설정된 경우 'HTTPS'프로토콜을 이용하여 통신하는 경우에만 쿠키를 전송할 수 있습니다.**

#### HttpOnly

자바스크립트에서 브라우저의 쿠키에 접근 여부를 결정합니다.
만약 해당 옵션이 `true`인 경우, 자바스크립트에서는 쿠키에 접근이 불가합니다.
기본값은 `false`이고 이 옵션이 `false`인 경우 자바스크립트에서 쿠키에 접근이 가능하므로 'XSS' 공격에 취약합니다.

#### SameSite

Cross-Origin 요청을 받은 경우 요청에서 사용한 메소드와 해당 옵션의 조합으로 서버의 쿠키 전송 여부를 결정하게 됩니다. 사용 가능한 옵션은 다음과 같습니다.

- **Lax:** Cross-Origin요청이면 'GET'메소드에 대해서만 쿠키를 전송할 수 있습니다.
- **Strict:** Cross-Origin이 아닌 `same-site`인 경우에만 쿠키를 전송할 수 있습니다.
- **None:** 항상 쿠키를 보내줄 수 있습니다. 다만 쿠키 옵션 중 Secure옵션이 필요합니다.
  이 때 `same-site`는 요청을 보낸 Origin과 서버의 도메인이 같은 경우를 말합니다.

> 이러한 옵션들을 지정한 다음 서버에서 클라이언트로 쿠키를 처음전송하게 된다면 헤더에 `Set-Cookie`라는 프로퍼티에 쿠키를 담아 쿠키를 전송하게 됩니다. 이후 클라이언트 혹은 서버에서 쿠키를 전송해야 한다면 클라이언트는 헤더에 `Cookie`라는 프로퍼티에 쿠키를 담아 서버에 쿠키를 전송하게 됩니다.

## Cookie를 이용한 상태 유지

이러한 쿠키의 특성을 이용하여 서버는 클라이언트에 인증정보를 담은 쿠키를 전송하고, 클라이언트는 전달받은 쿠키를 요청과 같이 전송하여 Stateless한 인터넷 연결을 Stateful하게 유지할 수 있습니다. \*\*하지만 기본적으로는 쿠키는 오랜 시간 동안 유지될 수 있고, 자바스크립트를 이용해서 쿠키에 접근할 수 있기 때문에 쿠키에 민감한 정보를 담는 것은 위험합니다.

## Cookie 동작방식

![](https://images.velog.io/images/blackdavil01/post/c857b4ae-48ad-46a2-a22c-291bc03ebda1/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7,%202021-09-10%2012-05-09.png)

1. 클라이언트가 서버에 로그인 요청을 합니다.
2. 서버는 클라이언트의 로그인 요청의 유효성을 확인하고(아이디와 비밀번호 검사)
   응답헤더에 `Set-cookie: user=chrisjune`를 추가하여 응답합니다.
3. 클라이언트는 이후 서버에 요청할 때 전달받은 `cookie: user=chrisjune`쿠키를 자동으로 요청헤더에 추가하여 요청합니다. 헤더에 쿠키값을 자동으로 추가하여 주는데 이는 브라우저에서 처리해주는 작업입니다.

# Session

## Session이란?

세션이란 사용자가 인증에 성공한 상태, 방문자가 웹 서버에 접속해 있는 상태를 하나의 단위로 보고 그것을 세션이라고 합니다.
즉 웹 브라우저를 통해 서버에 접속한 이후부터 브라우저를 종료할 때까지 유지되는 상태이다.

## Session 옵션

- **secret:** 보안을 위한 임의의 문자열(secret key)
- **resave:** 세션 데이터가 바뀌기 전까지 세션저장소의 값을 저장할 지 여부(default: false)
- **saveUninitialized:** 세션이 필요하기 전에 세션을 구동할지 여부(default:true)
- **store:** 세션저장소를 지정

## Session 동작방식

![](https://images.velog.io/images/blackdavil01/post/b25595bc-0591-462b-b411-10dd8084b716/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7,%202021-09-10%2021-22-31.png)

1. 클라이언트가 서버에 로그인 요청을 합니다.
2. 서버는 클라이언트의 로그인 요청의 유효성을 확인하고(아이디와 비밀번호 검사)유니크한 id를 session_id라는 이름으로 저장합니다.
3. 서버가 응답할 때 **응답헤더**에 `Set-cookie: session_id:12e3r`를 추가하여 응답합니다.
4. 클라이언트는 이후 서버에 요청할 때 전달받은 `session_id:12e3r`쿠키를 자동으로 **요청헤더**에 추가하여 요청합니다.
5. 서버에서는 요청헤더의 `session_id```값을 저장된 세션저장소에서 찾아보고 유효한지 확인후 요청을 처리하고 응답합니다.

# 쿠키와 세션의 차이점

#### 1. 저장위치

- 쿠키: 클라이언트
- 세션: 서버

#### 2. 보안

- 쿠키: 클라이언트에 저장되므로 보안에 취약하다
- 세션: 쿠키를 이용해 Session_id만 저장하고 이 값으로 구분해서 서버에서 처리하므로 비교적 보안성이 좋다.

#### 3. 라이프사이클

- 쿠키: 만료시간에 따라 브라우저를 종료해도 계속해서 남아 있을 수 있다.
- 세션: 만료시간을 정할 수 있지만 브라우저가 종료되면 만료시간에 상관없이 삭제된다.

#### 4. 속도

- 쿠키: 클라이언트에 저장되어서 서버에 요청 시 빠르다.
- 세션: 실제 저장된 정보가 서버에 있으므로 서버의 처리가 필요해 쿠키보다 느리다.

> 💡 깨알메모
> express를 사용하여 구현할때
> res.cookie('key', 'value', options) => 쿠키 생성(Set-cookie에 key와 value값으로 쿠키가 생성됨)
> req.cookies.쿠키이름 => 쿠키 조회가능

**Reference**

- [https://chrisjune-13837.medium.com/web-%EC%BF%A0%ED%82%A4-%EC%84%B8%EC%85%98%EC%9D%B4%EB%9E%80-aa6bcb327582](https://chrisjune-13837.medium.com/web-%EC%BF%A0%ED%82%A4-%EC%84%B8%EC%85%98%EC%9D%B4%EB%9E%80-aa6bcb327582)

- [https://doooyeon.github.io/2018/09/10/cookie-and-session.html](https://doooyeon.github.io/2018/09/10/cookie-and-session.html)

- [https://hahahoho5915.tistory.com/32](https://hahahoho5915.tistory.com/32)
