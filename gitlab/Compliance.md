### 정책관리
- 자격 증명 인벤토리
    - GitLab 인스턴스에서 모든 사용자가 사용하는 자격 증명을 추적합니다.
- 세분화된 사용자 역할 및 유연한 권한
    - 다섯 가지 다른 사용자 역할과 외부 사용자를 위한 설정으로 액세스 및 권한을 관리합니다. 사용자의 역할에 따라 권한을 설정하고, 리포지터리에 대한 읽기 또는 쓰기 액세스가 아닌 역할에 따라 권한을 설정합니다.
- Merge Request 승인 정책
- 푸시 규칙
- 보호된 브랜치를 사용해 임무를 분리
    - GitLab 크로스 프로젝트 YAML 구성을 활용해 코드의 배포자와 코드 개발자를 정의
- 보안 정책
    - 프로젝트 파이프라인에서 보안 스캐너를 실행하도록 구성
### 워크플로 자동화
- 컴플라이언스 프레임워크
- 컴플라이언스 파이프라인(ci/cd)
- Merge Request 승인 정책, 승인 설정
### 감사 관리
- 감사 이벤트: 코드 무결성 관리, 변경사항 제어/분석/추적할 수 있도록 도와줌.
    - 감사 이벤트는 데이터베이스에 저장되며, 감사 이벤트 보고서(대시보드)나 API로 조회할 수 있다. 
    - 감사 이벤트 API: https://docs.gitlab.com/ee/api/audit_events.html
- 감사 보고서: 감사 이벤트를 기반으로 보고서를 생성함.
### 기타 기능 
- 사용자 이메일 발송
- Tos 수락 강제하기 (Terms of Service: 서비스 이용 약관)
- 외부 상태 확인
- 사용자 권한 수준의 보고서 생성
- 라이선스 승인 정책
- 그룹 내 프로젝트 멤버 추가를 막기
- LDAP 그룹 동기화
- LDAP 그룹 동기화 필터
- Linux 패키지 설치 지원 로그 전달
- SSH 키 제한 

### 컴플라이언스 파이프라인 
- 프로젝트 파이프라인 설정과 별개로 컴플라이언스 파이프라인 설정을 할 수 있다.
- 프로젝트 파이프라인은 하위설정을 오버라이드 하지 않도록 컴플라이언스 파이프라인 설정 맨 위에 포함되어야 한다.
#### 컴플라이언스 파이프라인 설정 
-  Secure > Compliance Center > Frameworks > New Framework > 컴플라이언스 ci yml 파일을 추가한다.
#### 예제
```yml
include:  # 프로젝트에 .gitlab-ci.yml이 포함된 경우 개별 프로젝트 구성 실행
  - project: '$CI_PROJECT_PATH'
    file: '$CI_CONFIG_PATH'
    ref: '$CI_COMMIT_SHA' # 정의되어야 하며, 그렇지 않으면 MR 파이프라인에서는 항상 기본 브랜치를 사용
    rules:
      - if: $CI_PROJECT_PATH != "my-group/project-1" # 이 구성을 호스팅하는 프로젝트가 아닌 다른 프로젝트에서 실행되어야 함.

# 컴플라이언스 팀이 스테이지/작업의 순서와 꼬임 제어를 할 수 있게 합니다.
# 작업이 정의되지 않은 스테이지는 숨겨집니다.
stages:
  - pre-compliance
  - build
  - test
  - pre-deploy-compliance
  - deploy
  - post-compliance

variables:  # 프로젝트의 로컬 .gitlab-ci.yml에서 작업별 변수를 설정하여 재정의할 수 있습니다.
  FOO: sast

sast:  # 이러한 속성을 프로젝트의 로컬 .gitlab-ci.yml에서 재정의할 수 없습니다.
  variables:
    FOO: sast
  image: ruby:2.6
  stage: pre-compliance
  rules:
    - if: $CI_COMMIT_BRANCH && $CI_OPEN_MERGE_REQUESTS && $CI_PIPELINE_SOURCE == "push"
      when: never
    - when: always  # 또는 when: on_success
  allow_failure: false
  before_script:
    - "# No before scripts."
  script:
    - echo "running $FOO"
  after_script:
    - "# No after scripts."

sanity check:
  image: ruby:2.6
  stage: pre-deploy-compliance
  rules:
    - if: $CI_COMMIT_BRANCH && $CI_OPEN_MERGE_REQUESTS && $CI_PIPELINE_SOURCE == "push"
      when: never
    - when: always  # 또는 when: on_success
  allow_failure: false
  before_script:
    - "# No before scripts."
  script:
    - echo "running $FOO"
  after_script:
    - "# No after scripts."

audit trail:
  image: ruby:2.7
  stage: post-compliance
  rules:
    - if: $CI_COMMIT_BRANCH && $CI_OPEN_MERGE_REQUESTS && $CI_PIPELINE_SOURCE == "push"
      when: never
    - when: always  # 또는 when: on_success
  allow_failure: false
  before_script:
    - "# No before scripts."
  script:
    - echo "running $FOO"
  after_script:
    - "# No after scripts."
```

참고: 
- https://gitlab-docs.infograb.net/ee/administration/compliance.html#%EC%A0%95%EC%B1%85-%EA%B4%80%EB%A6%AC
- https://docs.gitlab.com/ee/user/group/compliance_pipelines.html
