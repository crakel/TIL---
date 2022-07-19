# SQL Injection

## SQL Injection?

코드 인젝션의 한 기법으로 악의적인 사용자가 보안상의 취약점을 이용하여, 클라이언트의 입력값을 조작해 임의의 SQL 문을 주입하고 실행되게 하여 데이터베이스가 비정상적인 동작을 하도록 조작하는 행위


## 인증 우회 방식

<img src = "https://slidesplayer.org/slide/16486568/96/images/7/SQL+INJECTION+SQL+%EB%8F%99%EC%9E%91+%EC%9B%90%EB%A6%AC%EB%A5%BC+%EC%9D%B4%EC%9A%A9%ED%95%9C+%EC%9D%B8%EC%A6%9D+%EC%9A%B0%ED%9A%8C.jpg">


SQL 인젝션 공격의 대표적인 경우로, 로그인 폼(Form)을 대상으로 공격을 수행한다. 정상적인 계정 정보 없이도 로그인을 우회하여 인증을 획득할 수 있다.

ex.
```SQL
SELECT * FROM USER WHERE ID = "abc" AND PASSWORD = "1234";
```

위와 같은 방식으로 로그인 시 쿼리가 처리 된다면,

```SQL
"1234"; DELETE * USER FROM ID = "1";
```
와 같이 동시에 다른 쿼리문을 함께 입력해
데이터베이스에 영향을 주거나 admin 계정을 탈취할 수 있다.

ex2.
```SQL
SELECT * FROM USER WHERE ID = "abc" AND PASSWORD = ' or '1'='1
```
기본 쿼리문의 WHERE 절에 OR문을 추가하여 '1' = '1'과 같은 true문을 작성하여 무조건 적용되도록 수정한 뒤 DB를 마음대로 조작할 수도 있다.


## 데이터 노출 방식
시스템에서 발생하는 에러 메시지를 이용해 공격하는 방법이다. 보통 에러는 개발자가 버그를 수정하는 면에서 도움을 받을 수 있지만, 해커들은 이를 역이용해 악의적인 구문을 삽입하여 에러를 유발시킨다.


ex.
```
http://test.com/login?id=1234 and password=''
```

 GET 방식으로 동작하는 URL에 쿼리 스트링을 추가하여 에러를 발생시킨다. 이에 해당하는 오류가 발생하면, 이를 통해 해당 웹앱의 데이터베이스 구조를 유추할 수 있고 해킹에 활용한다.

url을 통해 파라미터를 주고받는 GET 방식은 해커가 단순히 url을 통해 전달될 파라미터를 조작하기만 한다면 손쉽게 SQL injection 취약점을 적용할 수 있다.

## 방어 방법

### 입력값 검증
검증 로직을 추가하여 미리 설정한 특수문자들이 들어왔을 때 요청을 막아낸다.

### Error Message 노출 금지
DB에러에 대한 메세지를 따로 처리해 테이블명, 컬럼명, 쿼리문 등 DB구조를 파악할 수 있는 메세지를 노출되지않도록 한다.

### Prepared Statement 구문사용
ex.
```SQL
INSERT INTO MyGuests VALUES(?, ?, ?)
```

외부 입력으로 수정할 수 없는 템플릿을 사용

?에 해당하는 데이터는 문자열로 취급되고 서버 내부에서 처리되기 때문에 SQL 인젝션을 방지할 수 있다.


