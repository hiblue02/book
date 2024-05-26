## Performance 스키마
MySQL 서버가 기본적으로 제공하는 시스템 데이터베이스 중 하나, `performance_schema`
MySQL 서버 *내부 동작 및 쿼리 처리*와 관련된 세부 정보들이 저장되고, 이 테이블들을 통해 MySQL 서버의 성능을 분석하고 내부 처리 과정을 모니터링 할 수 있다. 
### SetUp 테이블
- Performance 스키마의 데이터 수집 및 저장과 관련된 설정 정보가 저장돼있다. 
- setup_actors, setup_consumers, setup_instruments, setup_objects, setup_threads
### Instance 테이블
- cond_instances: 현재 mysql 서버에서 동작하는 스레드들이 대기하는 조건(Condition) 인스턴스들의 목록
- file_instances, mutex_instances, rwloc_instances(읽기 및 쓰기 잠금 인스턴스들의 목록), socket_instances
### Connection 테이블
- MySQL에서 생성된 커넥션들에 대한 통계 및 속성 정보를 제공한다. 
- accounts, hosts, users, session_account_connect_attrs, session_connect_attrs
### Variable 테이블
- 서버의 시스템 변수 및 사용자 정의 변수와 상태 변수에 대한 정보를 제공한다. 
- global_variables, session_variables, variables_by_thread, persisted_variables, variables_info, user_variables_by_thread, gloabl_status, session_status, status_by_thread
### Event 테이블
- 스레드에서 실행된 쿼리 처리와 관련된 이벤트
- 이벤트 테이블은 Wait, Stage, Statement, Transcation으로 구분되어 있다. (Transaction > Statement > Stage > Wait)
  - wait: 각 스레드에서 대기하고 있는 이벤트들에 대한 정보 
  - stage: 각 스레드에서 실행안 쿼리들의 처리단계
  - statement: 각 스레드에서 실해앙 쿼리에 대한 정보 
  - transaction: 각 스레드에서 실행한 트랜잭션에 대한 정보  
- 각 이벤트는 세가지 유형이 테이블을 갖는다.
  - current: 스레드별로 가장 최신 이벤트 1건
  - history: 스레드별 가장 최신 이벤트가 지정된 최대 갯수만큼 저장됨
  - history_log: 전체 스레드에 대한 최근 이벤트를 모두 저장 지정된 최대 갯수만큼 저장됨.
  - ### Summary 테이블
- Performance 스키마가 수집한 이벤트들을 특정 기준별로 집계한 후 요약한 정보를 제공한다. 
- events_transactions_summary_global_by_event_name (이벤트 클래스로 분류해서 집계한 Transaction 이벤트 통계정보)
- objects_summary_global_by_type (데이터베이스 객체별로 분류해서 집계한 대기 시간 통계 정보)
- table_io_waits_summary_by_table (테이블별로 분류해서 집계한 IO 작업 관련 소요시간 통계 정보)
- table_lock_waits_summary_by_table (테이블별로 분류해서 집계한 잠금 종류별 대기 시간 통계 정보)
- memory_summary_by_global_by_event_name (이벤트 클래스별로 분류해서 집계한 메모리 할당 및 해제에 대한 통계 정보)
- events_errors_global_summary_global_by_error (에러 코드별로 분류해서 집계한 MySQL 에러 발생 및 처리에 대한 통계 정보)
### Lock  테이블
- data_locks (현재 잠금이 점유됐거나, 잠금이 요청된 상태에 있는 데이터 관련 락 정보)
- data_lock_waits (이미 점유된 데이터 락과 이로 인해 잠금 요청이 차단된 데이터 락 간의 관계 정보)
- metadata_locks (현재 잠금이 점유된 메타데이터 락에 대한 정보)
- table_handles (현재 작므이 점유된 테이블 락에 대한 정보)
### Replication 테이블
- 복제 연결과 관련된 설정, 상태 정보
### Clone 테이블 
- Clone 플러그인을 통해 수행되는 복제 작업에 대한 정보 
### 기타 테이블 
- error_log (MySQL 에러 로그 파일)
- host_cache (MySQL의 호스트 캐시 정보)
- keyring_keys (MySQL의 Keyring 플러그인에서 사용되는 정보)
- log_status (MySQL 서버 로그 포지션 정보, 온라인 백업 때 활용)
- performance_timers (Performance 스키마에서 사용 가능한 이벤트 타이머들과 그 특성에 대한 정보)
- processlist (연결 세션 목록과 세션의 현재 상태 = SHOW PROCESSLIST)
- threads (MySQL 서버 내부의 백그라운드 스레드, 포그라운드 스레드 정보)
- tls_channel_status (연결 인터페이스별 TLS(SSL) 속성 정보)
- user_defined_functions (컴포넌트나 플러그인에 의해 자동으로 등록됐거나, create function 명령문에 의해 생성된 사용자 정이 함수들에 대한 정보)
## Sys 스키마
### sys_config 
- Sys 스키마의 함수 및 프로시저에서 참조되는 옵션들이 저장돼 있는 테이블
### 뷰
- Formatted-View: 사람이 쉽게 읽을 수 있는 수치로 변환해서 보여주는 뷰 
- Raw-View: x$로 시작하며, 저장된 원본 형태 그대로 출력해서 보여준다. 
- processlist(현재 실행중인 스레들에 대한 정보)
- schema_auto_increment_columns(auto_increment 칼럼이 존재하는 테이블에 대한 현재 및 최대값 정보)
- schema_object_view (데이터베이스별로 해당 데이터베이스에 존재하는 객체 유형별 개체 수 정보)
- schema_redunant_indexs (포함이나 중복 관계인 인덱스를 출력한다.)
- schema_index_statics (인덱스 통계 정보)
- schema_table_statics (테이블 작업별 수행횟수, 지연시간, IO발생량 등의 통계 정보)
- schema_unused_indexes (MySQL 서버가 구동중인 기간 동안 테이블에서 사용되지 않은 인덱스들의 목록이 출력된다.)
- statement_analysis (수행된 전체 쿼리에 대한 통계정보)
- wait_classes_global_ba_latency(Wait 이벤트 통계정보)

### sys 테이블 사용 예시 
#### 변경이 없는 테이블 목록 확인
```SQL
select t.table_schema ,, t.table_name, t.table_rows, tio.count_read, tio.count_write
from information_schema.tables AS t 
join performance_schema.table_io_waits_summary_by_table as tio
on t.table_schema = tio.object_schema and t.object_name = tio.table_name
where t.table_schema not in ('mysql', 'performance_schema', 'sys')
and tio.count_write = 0
order by t.table_schema, t.table_name;
```
#### 요청이 많은 테이블 목록 확인
```SQL
select * from sys.io_global_by_file_bytes where file like '%bd';
```
#### 테이블별 작업량 통계 확인
```SQL
select table_schema, rows_fetched, rows_inserted, rows_updated, io_read, io_write
from sys.schema_table_statistics 
where table_schema not in ('mysql', 'performance_schema', 'sys')
```
#### 자주 실행되는 쿼리 목록 확인
```SQL
select db, exec_count, query
from sys.statement_analysis
order by exec_count desc;
```
#### 실행시간이 긴 쿼리 목록 확인
```SQL
select db, exec_count, sys.format_time(avg_latency) as 'formatted_avg_latency', rows_sent_avg, rows_examined_avg, last_seen
from sys.x$statement_analysis
order by avg_latency desc;
```
