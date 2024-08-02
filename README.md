# comento-3
# 3주차

# 3-1.스프링부트로 개발 환경 설정하기
스프링부트 개발 횐결 설정하기
<img width="361" alt="스크린샷 2024-07-26 오후 7 10 32" src="https://github.com/user-attachments/assets/85879b2e-abdd-4bfc-b240-089fbbba8802">


# 3-2. 통계(SW활용현황) API를 위한 DB, Table 생성
statistic13 DB를 생성한 후에 requestInfo, requestCode, user table을 생성한 후에 requestInfo에 데이터 삽입
<img width="491" alt="스크린샷 2024-07-26 오후 8 19 44" src="https://github.com/user-attachments/assets/ed85c23b-ed6f-4935-a1dc-908531ea0df4">

<img width="717" alt="스크린샷 2024-07-26 오후 8 19 50" src="https://github.com/user-attachments/assets/b6d86c4e-21a1-45d2-8e14-a3d9be05c79e">

# 3-3. [년도 로그인수 API ]스프링부트, Mybatis, mariadb 연동

<img width="380" alt="image" src="https://github.com/user-attachments/assets/43ebbf5c-38b2-4a6a-9bca-6028a858138a">

<img width="1493" alt="image" src="https://github.com/user-attachments/assets/754fa29e-829f-4ba4-ac70-f8ccb429920f">

<img width="1440" alt="image" src="https://github.com/user-attachments/assets/28325299-63e5-4a0d-8993-8dfb9b854378">

오류의 원인을 못찾겠습니다

## 해결

<img width="401" alt="스크린샷 2024-08-02 오후 8 30 19" src="https://github.com/user-attachments/assets/3b289c46-aed8-438a-a7c2-f704e1440efa">
<img width="423" alt="스크린샷 2024-08-02 오후 8 30 30" src="https://github.com/user-attachments/assets/de91d391-1d33-498d-bc3a-b298e873652d">
<img width="379" alt="스크린샷 2024-08-02 오후 8 32 51" src="https://github.com/user-attachments/assets/bcdec3cd-e2d7-4092-928f-4b97c74b5892">


# SW활용 현황 통계 API 구축을 위한 SQL 작성
## 1.월별 접속자 수
 ```
SELECT
    DATE_FORMAT(STR_TO_DATE(createDate, '%y%m%d%H%i'), '%Y-%m') AS month,
    COUNT(DISTINCT userID) AS login_count
FROM
    statistic13.requestInfo
GROUP BY
    DATE_FORMAT(STR_TO_DATE(createDate, '%y%m%d%H%i'), '%Y-%m')
ORDER BY
    month;
 ```
## 2. 일자별 접속자 수
 ```
SELECT
    DATE(STR_TO_DATE(createDate, '%y%m%d%H%i')) AS date,
    COUNT(DISTINCT userID) AS login_count
FROM
    statistic13.requestInfo
GROUP BY
    DATE(STR_TO_DATE(createDate, '%y%m%d%H%i'))
ORDER BY
    date;
 ```
## 3. 평균 하루 로그인 수
 ```
SELECT
    AVG(daily_logins) AS average_daily_logins
FROM (
    SELECT
        DATE(STR_TO_DATE(createDate, '%y%m%d%H%i')) AS date,
        COUNT(*) AS daily_logins
    FROM
        statistic13.requestInfo
    GROUP BY
        DATE(STR_TO_DATE(createDate, '%y%m%d%H%i'))
) AS daily_counts;
 ```
## 4. 휴일을 제외한 로그인 수
휴일 테이블을 별도로 생성하여 휴일에 해당하는 DATE를 삽입한다.
```
CREATE TABLE statistic13.holidays (
    holiday_date DATE NOT NULL PRIMARY KEY
);

-- 휴일을 제외한 로그인 수를 계산하는 쿼리
SELECT
    DATE(STR_TO_DATE(createDate, '%y%m%d%H%i')) AS date,
    COUNT(DISTINCT userID) AS login_count
FROM
    statistic13.requestInfo
WHERE
    DATE(STR_TO_DATE(createDate, '%y%m%d%H%i')) NOT IN (SELECT holiday_date FROM statistic13.holidays)
GROUP BY
    DATE(STR_TO_DATE(createDate, '%y%m%d%H%i'))
ORDER BY
    date;
```
## 5. 부서별 월별 로그인 수
```
SELECT
    u.HR_ORGAN AS department,
    DATE_FORMAT(STR_TO_DATE(r.createDate, '%y%m%d%H%i'), '%Y-%m') AS month,
    COUNT(DISTINCT r.userID) AS login_count
FROM
    statistic13.requestInfo r
JOIN
    statistic13.user u ON r.userID = u.userID
GROUP BY
    u.HR_ORGAN,
    DATE_FORMAT(STR_TO_DATE(r.createDate, '%y%m%d%H%i'), '%Y-%m')
ORDER BY
    department,
    month;

```
