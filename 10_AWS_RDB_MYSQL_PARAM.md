## AWS RDS parameter group을 이용해 character-set 변경(utf8), 타임존 변경하기

#### 1.파라미터 그룹 생성
#### 2.파라미터 이름 생성
#### 3.검색에 character-set 검색하여, 다음과 같이 변경
* character_set_client : utf8
* character_set_connection : utf8 
* character_set_database : utf8
* character_set_filesystem : utf8
* character_set_results : utf8
* character_set_server : utf8

#### 4.검색에 collation 검색하여 다음과 같이 벼경
* collation_connection : utf8_unicode_ci
* collation_server : utf8_unicode_ci

#### 5.생성된 RDB 콘솔에 다음과 같이 프로시져 생성
```
DELIMITER |
CREATE PROCEDURE '설치한 DB명'.`store_time_zone`()
    IF NOT (POSITION('rdsadmin@' IN CURRENT_USER()) = 1) THEN
        SET SESSION time_zone = 'Asia/Seoul';
    END IF |

DELIMITER ;
```

#### 6. 다시 AWS RDB 파라미터 그룹에 생성한 파라미터에서 init_connect 로 검색하여 value 값 등록
```
CALL (설치한 DB명).store_time_zone
```


#### 7. 설정한 타임존 확인
```
select now(); 
```
