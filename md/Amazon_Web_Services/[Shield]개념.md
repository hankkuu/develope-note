# Shield

## DDos 공격

* Network, Transport, Application 레이어에 공격
* 디도스 공격의 트랜드
  * 물량 기반 65%
  * 어플리케이션 레이어 18%
  * 상태 소진형 17%



### Network 레이어로의 공격

* 물량 기반 디도스 공격
  * 정상적으로 처리할 수 있는 수준을 상회하는 트래픽을 전송하여 네트웍 기능을 마비시킴
  * ex) UDP reflection attacks, NTP reflection, DNS reflection, Chargen reflection, SNMP reflection
* SSDP reflection 공격이 가장 흔한 유형
  * Reflection 공격은 분명한 시그니처가 있으며, 가용 밴드위쓰를 전부 점유하는 형태



### Transport 레이어로의 공격

* 상태 소진형 디도스 공격
  * 프로토콜 특성을 악용하여 방화벽, IPS, 로드밸런서 같은 시스템을 무력화
  * ex) TCP SYN flood
    * 특정 타겟 서버로 특정 시간동안 많은 양의 TCP SYN 패킷을 보내서 TCP 커넥션 풀을 소진시킴



### Application 레이어로의 공격

* 어플리케이션 레이어 디도스 공격
  * 정상 요청으로 가장하지만, 방어수단을 우회하고 어플리케이션 리소스를 소진하기 위한 악의적인 요청을 통한 공격
  * ex) HTTP GET, DNS query floods, Slowloris
* 방어하기가 상당히 난해함.
* 점점 증가하는 추세





## 디도스 공격 대응의 어려움

### 적용이 어려움

* 복잡한 구성절차
* 충분한 밴드위쓰 확보
* 어플리케이션 아키텍쳐 재구성



### 수작업 대응 과정

* 공격 대응에 필요한 관계자 참여 과정
* 원격에 있는 정제장소를 경유토록 트래픽 라우팅 변경
  * 트래픽 라우팅 변경 = 사용자 지연 시간 증가
* 대응 시간의 증가



### 비싼 사용료

* 솔루션도 고가
* 적용하기 위한 노력도 많이 필요



## 디도스 방어를 위한 AWS의 접근 방법

* [DDos 대응 관련 백서](https://d0.awsstatic.com/International/ko_KR/whitepapers/DDoS_White_Paper.pdf)를 제공
* 디도스 공격이 발생했을 경우 우왕자왕 하지 않고 정해진 플랜에 따라 대응할 수 있도록 플랜을 잘 준비하는 것이 중요



#### AWS가 지향하는 목표

* 가용성에 대한 확신
* 획일적인 대규모 변경 필요성 제거
  * 자동화된 보호



#### AWS에 적용된 디도스 방어체계

* 빌트인으로 적용되어 있고 상시 활성화 되어 있음
  * AWS로 들어오는 모든 트래픽들은 인라인 모드로 점검이 된다.
  * 모든 패킷들은 디도스 여부를 판정 받고 사용자의 워크로드로 전달
* AWS의 방대하게 구성된 AWS 네트워크를 활용해서 외부 라우팅 없이 신속하게 디도스를 방어
* 여분의 AWS 데이터 센터 인터넷 연결성
  * AWS의 모든 리전은 인터넷 커넥티비티를 리덴던트하게 운영
    * 하나의 통로가 막히더라도 다른 여분의 통로로 정상적인 서비스가 가능하도록 함
* AWS 리소스들의 최전방에 블랙와치라는 AWS의 디도스 대응 시스템이 존재
  * L3/4의 일반적인 공격들은 여기서 다 커버됨
  * ex) SYN/ACK Floods, UDP Floods, Refection attacks등
  * 별도 비용 없음



## AWS Sheild

## Standard Protection

* 위에서 설명한 내용들에 대한 부분을 무료로 제공
* L3/4 보호
  * 자동 탐지 및 대응
  * 가장 흔한 공격 유형에 대한 방어
    * SYN/UDP Floods
    * Reflection Attacks
  * AWS 플랫폼에 빌트인 되어 있음
* L7 보호
  * L7 디도스 공격 대응을 위해 WAF 사용
  * 셀프 서비스 및 사용량 과금
* 네트웍 플로우 모니터링 제공
* 기존에 기본 기능에서 Shield라는 이름으로 사용하는 이유
  * 서비스화 하여 지속적으로 업데이트와 개선해 나갈 것을 약속하는 의미
* 공격 통보 및 리포팅을 따로 해주지 않음.
  * 공격이 있었던 것을 인지하지 못하고 넘어가는 경우도 많음.





## Advanced Protection

* 비용 발생
* 추가적인 보호와 기능 및 이점을 제공
* 다음 서비스들에 적용 가능
  * ALB
  * ELB (Classic Load Balancer)
  * CloudFront
  * Route 53
* 아직 서울 리전 지원 안함
  * 가장 가까운 리전은 도쿄
* 네트웍 플로우 모니터링 제공
* 어플리케이션 트래픽 모니터링 제공
* 공격 통보 및 리포팅 제공
  * CloudWatch를 통해 통보
  * 준 실시간 메트릭과 공격 분석을 위한 패킷 캡쳐
  * 과거 공격 이력 리포트
* 디도스 공격으로 인한 당시의 리소스 비용이 폭주하는 경우 이를 인지하여 비용에 청구하지 않음





## 특징

* AWS 연계성

  * 인프라 구성을 변경할 필요 없이 디도스 방어 기능 적용 가능
* 상시 탐지 및 대응
  * 어플리케이션 지연시간의 영향 최소화
* 적절성
  * 비용과 가용성 간에 저울질할 필요 없음
* 유연성
  * 사용자의 어플리케이션에 대한 맞춤식 보호




## 상시 모니터링 및 탐지

### 시그니처 기반 탐지

* SSDP와 같은 명백한 시그니처를 가지고 있는지 검사
  * 비교적 심플



### 휴리스틱 기반 비정상 상태 탐지

* 확률적으로 대응
  * 공격일지 아닐지 애매모호 할때 어떻게 대처할 것인지
* 변화의 추이를 따짐
* 다음과 같은 속성을 기반으로 비정상 상태 탐지
  * 소스 IP
  * 소스 ASN
  * Traffic Levels
  * 검증된 소스



### 기본 패턴 비교

* 고객의 워크로드 별로 프로파일링 된 데이터로 이상 징후 탐색
* 평상시 트래픽 패턴을 기준으로 상시 비교
  * 초당 HTTP 요청 수
  * 소스 IP 주소
  * URLs
  * User-Agents



## 참고

* [AWS Shield를 통한 DDoS 대비 복원성 강한 AWS 보안 아키텍처 구성](https://www.youtube.com/watch?v=aetVvzrumIQ)
* ​