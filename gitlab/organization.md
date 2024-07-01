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

### 이용자 그룹 만들기 
1. Your Work / Groups / NewGroup / Create Group으로 이동한다. 


### 참고문서
- https://docs.gitlab.com/ee/user/namespace/
- https://docs.gitlab.com/ee/user/reserved_names.html#limitations-on-usernames-project-and-group-names
