### Groups
- 그룹은 이용자 묶음으로 프로젝트 접근, 이슈, mr에 대한 권한을 그룹 단위로 설정할 수 있다. 그룹의 활동 분석자료도 볼 수 있다.
#### 그룹 관리
1. 그룹별로 default 브랜치에 대한 보호를 설정할 수 있다.
2. 그룹의 최대 인원을 설정할 수 있다. User Cap도 설정할 수 있다. (관리자 승인 없이 그룹멤버가 초대할 수 있는 인원 수)
3. 그룹 멘션이나, 이메일 발송 등을 제한할 수 있다.
4. Group File Template을 설정할 수 있다. (그룹 내 모든 프로젝트가 일관된 파일 구조와 설정을 가질 수 있다. 특정 그룹이 자주 사용하는 코드 파일, 문서 등을 그룹 파일 템플릿으로 설정한다.)
5. 그룹 레벨로 pipeline이 성공해야 merge할 수 있도록 강제할 수 있다. (Settings > General > Merge Requests > Merge Check / Pipelines must succeed)
6. mr에서 리뷰 쓰레드가 모두 resolved 되어야 merge될 수 있도록 제한할 수 있다. (https://docs.gitlab.com/ee/user/group/manage.html#prevent-merge-unless-all-threads-are-resolved)
7. 그룹 내에서 최근 90일 이내, 얼마나 많은 mr, 이슈, member가 생성되었는지 볼 수 있다. (https://docs.gitlab.com/ee/user/group/manage.html#group-activity-analytics)
8. 그룹 별 push rule을 관리할 수 있다. (https://docs.gitlab.com/ee/user/group/access_and_permissions.html#group-push-rules)

#### Group-Level Project Templates
[https://velog.io/@yeguu037/Git-GitLab-MR-Template-만들기](https://velog.io/@ss-won/Git-GitLab-Issue-MR-Template-%EB%A7%8C%EB%93%A4%EA%B8%B0)
https://gitlab-docs.infograb.net/ee/user/project/description_templates.html
