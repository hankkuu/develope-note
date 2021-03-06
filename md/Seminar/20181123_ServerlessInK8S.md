# Serverless in K8S

* 서버리스란?
  * BaaS나 FaaS, 혹은 둘 다를 이용한 어플리케이션 아키텍처 설계 방법론
  * AWS의 대표적인 FaaS는 Lambda, BaaS는 S3, DynamoDB 등
* 사용자의 요청은 FaaS의 API Gateway를 거쳐 추상화된 Controller가 요청에 해당하는 Function을 실행하고, 필요 시 BaaS(내부 서비스나 데이터베이스)를 사용하여 응답을 사용자에게 반환



##  Serverless vs Serverless Functions vs FaaS

* Serverless는 사상
* Serverless Functions는 서버리스 사상으로 커스텀한 함수를 실행시킬 수 있도록 하는 것
* FaaS는 이러한 Serverless Functions를 각 클라우드 프로바이더가 각 환경에 적합한 서비스로 제공하는 것



##  MSA vs Serverless Functions

* 구분 짓기 애매
* MSA가 서비스 단위로 잘게 쪼갰다면 Serverless Functions는 더 잘게 쪼갬 (코드 조각)



##  서버리스 장점

* 저렴
* 서버 관리 불필요
* 확장성
* 작은 비즈니스에 유리
* 빠른 프로토타입



##  서버리스 단점

* 콜드 스타트
  * 첫 실행 시 컨테이너가 구동되는 시간이 필요
* Nano Service
  * 너무 잘개 쪼개져있어서 관리의 어려움
  * 안티패턴
* 서버리스도 서버가 있음
  * 사내에서 자체 구성하기 위해서는 서버리스 서비스를 제공하는 부서에서는 서버 관리가 필요
  * 클라우드 프로바이더가 제공하는 서비스는 서버 관리를 프로바이더에서 하기 때문에 서버리스라 할 수 있음



##  SK에서 활용

* Private FaaS를 위해 자체 구현
* 쿠버네티스 기반으로 구현
  * Fission 오픈소스
    * Serverless Functions Framework
    * CNCF에 등록된 오픈소스
    * 서버리스 출현이 근래이기 떄문에 대부분의 오픈소스가 인지도가 없었음
    * 콜드 부팅의 문제가 가장 적은 오픈소스를 선택
    * 함수를 생성/삭제/관리 할 수 있는 오픈소스
    * Fission Workflow 오픈소스
      * 너무 잘개 쪼개진 함수들의 관리가 어려운데 이를 묶어서 워크플로우를 생성해주는 프로젝트
  * Dispatch 오픈소스
    * 서버리스 프레임워크
    * Dispatch 위에 Fission이 올라가는 방식
* Private 할 필요가 없다면 퍼블릭 클라우드를 이용하는 것이 좋음
* 콜드 스타트 문제를 해결하기 위해 고생함
  * 프리웜을 수행하는 방식으로 해결
  * 요청 -> 런타임생성 -> 코드 설치 -> 의존성 설치 -> 함수 실행
  * Fission은 런타임생성까지 프리웜하는 것으로 콜드 스타트 개선
  * SK에서는 코드 설치까지 프리웜 진행
    * 코드 설치는 코드가 커밋되면 docker volume을 통해 실제 호스트에 동기화
  * 각 코드와 자원에 따른 컨테이너들을 미리 구동시켜놓고 재사용
    * 구동된 컨테이너는 콜드 스타트 문제 때문에 정해진 개수만큼은 풀로 관리
    * 함수 실행 요청이 끝나면 해당 함수에 대한 부분만 메모리에서 제거하는 방식으로 사용
    * 이전 코드로 인해 환경이 변경될 수 있는 위험 요소가 있음
      * 이를 위해 컨테이너를 재생성하는 전략이 필요
    * 사용자의 니즈에 따라 컨테이너 지속성이 필요한 경우를 선택적으로 할 수 있도록 함
      * 선택에 따라 주기적으로 컨테이너가 종료되거나 유지를 결정
    * Fission내에 이러한 재사용에 대한 설정을 위해 3가지 타입을 제공
* 사용자 코드에 따른 부하 관리
  * 사용자 코드에 부하를 줄만한 코드가 있는지 검사하는 것은 불가능
  * 이를 검증하기 위해 노드를 분리
    * 미리 정상 범위의 메트릭을 산정하고 이를 초과하는 컨테이너들을 별도 관리
  * Management Worker Node
    * nodeSelector 방식
      * Pod이 노드를 선택해서 들어감
  * Runtime Worker Node
    * Taint & tolerations 방식
    * 이 방식을 사용하여 부하 분산
* 부하 관리
  * Docker는 현재 상태 지표를 파일로 저장함
    * /sys/fs/cgroup
  * 파일의 정보를 기반으로 부하를 많이 먹고 있는 사용자의 컨테이너를 제한



##  Tip

* Helm의 업데이트 히스토리 관리가 어려워서 Helm은 Template를 생성하는 용도로만 사용하고 이를 통해 나온 yaml 파일을 kubectl apply 명령으로 실행
  * kubectl apply 명령은 히스토리 관리가 가능

