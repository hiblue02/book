### 프로젝트 생성
참고: https://gitlab-docs.infograb.net/ee/user/project/
1. 왼쪽 사이드바 상단에서 새로 만들기 (+)
2. 템플릿에서 만들기 or 빈 프로젝트 만들기

### 프로젝트 권한 구성 
참고: https://gitlab-docs.infograb.net/ee/user/project/settings/
1. 왼쪽 사이드바 **검색 또는 이동**을 선택
2. 설정 > 일반 > 가시성, 프로젝트 기능, 권한 열기 > 변경 후 저장

아래 기능을 비활성화하면 다음 추가기능도 함께 비활성화된다. 
- 이슈
    - 이슈 보드
    - 서비스 데스크
    - MR에서는 마일스톤에 엑세스 할 수 있다.
- 이슈 & MR
    - 라벨
    - 마일스톤
- 리포지터리
    - MR
    - CI/CD
    - 대용량 파일 스토리지 Git LFS
    - 패키지  

* 마일스톤 : https://insight.infograb.net/docs/user/milestone_management/

### 설명 템플릿
참고: https://gitlab-docs.infograb.net/ee/user/project/description_templates.html
이슈 및 MR에 사용할 템플릿을 정의할 수 있다. 
#### 설명 템플릿 파일
- 마크다운 `.md` 파일이어야 한다.
- 프로젝트의 리포지터리에 .gitlab/issue_templates 또는 .gitlab/merge_request_templates 디렉터리에 저장되어 있어야 한다.
    - 이슈 템플릿: .gitlab/issue_templates
    - MR 템플릿: .gitlab/merge_request_templates
- 기본 브랜치에 있어야 한다.
#### 템플릿 사용
- 이슈/MR을 생성/편집할 때 템플릿 선택 드롭다운 디렉토리에 표시된다.
#### MR 템플릿에서 지원되는 변수 
- 참고: [MR 템플릿에서 지원되는 변수](https://gitlab-docs.infograb.net/ee/user/project/description_templates.html#merge-request-%ED%85%9C%ED%94%8C%EB%A6%BF%EC%97%90%EC%84%9C-%EC%A7%80%EC%9B%90%EB%90%98%EB%8A%94-%EB%B3%80%EC%88%98)
#### 기본 템플릿 지정
- 이슈 기본 템플릿: 왼쪽 사이드바 > 검색 또는 이동 > 설정 > 일반 > 이슈에 대한 기본 설명 템플릿 > 변경 후 저장
- MR 기본 템플릿: 왼쪽 사이드바 > 검색 또는 이동 > 설정 > Merge Request > Merge Request에 대한 기본 설명 템플릿 > 변경 후 저장  

