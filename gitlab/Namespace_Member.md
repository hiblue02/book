### NameSpace
1. NameSpace는 여러 프로젝트를 묶는 단위이자, 프로젝트를 분리하는 단위다. (같은 이름의 프로젝트가 여러 NameSpace에 있을 수 있다.)
2. NameSpace의 종류
    - personal: 개인 사용자의 이름에 기반을 두며, 개인 계정을 만들때 부여된다.
        - subgroup을 만들 수 없다.
        - 사용자 이름을 바꾸면, namespace의 이름도 같이 바뀐다.
        - namespace 내부의 group들은 namespace의 권한을 상속받지 않는다.
        - 개인이 생성하는 모든 프로젝트는 이 namespace에 속한다.   
    - group/subgroup: 그룹이나, 서브그룹의 이름에 기반을 둔다.
        - 여러 개의 서브그룹을 만들 수 있다.
        - 서브그룹이나 프로젝트 각각 특별하게 설정할 수 있다.
        - 서브 그룹은 부모 그룹의 설정을 상속받는다.
        - 그룹이나 서브그룹 이름을 바꾸면 namespace 이름도 바뀐다.
3. 프로젝트 및 group 이름 규칙
   - 영어나 숫자로 시작해야 한다.
   - contain only letters (a-zA-Z), digits (0-9), emoji, underscores (_), dots (.), parentheses (()), dashes (-), or spaces.
   - `.git`, `.atom`으로 끝나면 안된다.
### Member
1. Member는 너의 프로젝트에 접근할 수 있는 user나 group이다.
2. Member의 자격
    - Direct: 현재 그룹이나 프로젝트에 직접 추가된 사용자
    - Indirect: 부모 그룹의 상속자거나, 다른 그룹에 의해 현재 그룹/프로젝트에 초대된 사용자
    - Inherited: 현재 그룹/프로젝트을 가진 부모 그룹의 멤버인 사용자
    - Shared: 현재나 조상 그룹/프로젝트에 초대된 사용자
    - Inherited shared: 현재 그룹/프로젝트에 초대된 부모 그룹/프로젝트의 멤버인 사용자
    - Member 자격에 따른 프로젝트 보기권한: https://docs.gitlab.com/ee/user/project/members/#membership-and-visibility-rights
3. Member 추가하기
    - direct member는 바로 Max Role과 만료기한을 부여할 수 있지만 inherited member는 부모 그룹에서 설정해야 된다. 
    - 직접 추가하기: Manage > Members > Invite Members
    - 다른 프로젝트의 멤버들을 집어넣기: Manage > Members > Import from a project
5. Member 삭제하기
    - direct member는 바로 삭제할 수 있지만, inherited member는 부모 그룹에서 삭제해야 된다.
    - Manage > Members > Remove Member > (선택사항: 관련된 이슈와 mr에서도 사용자 할당 해제) 
    - 삭제할 멤버가 개인 repository에 project를 fork하지 않았는지 확인해야 한다.
    - Mantainer/Owner 역할을 가진 악성 사용자는 자신을 다시 프로젝트/그룹에 초대할 수 있다. 이를 막기 위해 악성 사용자의 계정을 차단할 수 있다.
6. Project Access 요청하기
   - 접근하고 싶은 프로젝트를 찾아, 우측 상단의 `...`을 눌러 `request access`를 요청한다. 프로젝트 관리자가 승인하면 접근할 수 있다.
   - 프로젝트 관리자가 승인해주기 전에 요청을 회수할 수 있다. 프로젝트 이름 옆에 `Withdraw Access Request`를 선택한다. 
7. User 차단하기
   - Owner 역할을 가진 사용자는 어떤 사용자의 접근을 차단할 수 있다.
   - Settings > General > Visibility, project features, permissions > Users can request access > Save Change   
  
### 참고문서
- https://docs.gitlab.com/ee/user/namespace/
- https://docs.gitlab.com/ee/user/reserved_names.html#limitations-on-usernames-project-and-group-names
