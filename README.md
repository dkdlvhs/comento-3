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



# SW활용 현황 통계 API 구축을 위한 SQL 작성
## 1.월별 접속자 수
'''
SELECT
    DATE_FORMAT(STR_TO_DATE(createDate, '%y%m%d%H%i'), '%Y-%m') AS month,
    COUNT(DISTINCT userID) AS login_count
FROM
    statistic.requestInfo
GROUP BY
    DATE_FORMAT(STR_TO_DATE(createDate, '%y%m%d%H%i'), '%Y-%m')
ORDER BY
    month;
'''
