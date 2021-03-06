* 이번 장에서 다룰 내용
* 검색 엔진에 의해 처리되는 데이터의 특성
* 일반적인 검색 엔진의 사용 사례
* 솔라의 주요 구성 요소
* 솔라를 선택해야하는 이유
* 기능 개요

소셜 미디어, 클라우드 컴퓨팅, 모바일 애플리케이션 및 빅데이터와 같이 급성장하는 기술을 사용하는 것이 컴퓨팅에있어 흥미롭고 도전적인 시기이다. 소프트웨어 설계자가 직면 한 주요 과제 중 하나는 글로벌 사용자들이 소비하고 생산해내는 엄청난 양의 데이터를 처리하는 것이다. 게다가 사용자들은 온라인 응용 프로그램이 항상 사용 가능하고 반응 또한 좋을 것으로 기대한다. 최근 웹 응용 프로그램의 확장성 및 가용성 요구 사항을 해결하기 위해 SQL뿐만 아니라 NoSQL을 비롯한 특수한 비 관계형 데이터 저장소 및 처리 기술에 대한 관심이 증가하고 있다.

이 시스템들은 모든 데이터를 단일 표준 관계형 모델(the once-standard relational model)로 강제로드하는 대신 특정 유형의 데이터에 일치하는 저장 영역 및 처리 엔진의 공통 설계 패턴을 공유한다. 즉, NoSQL 기술은 특정 유형의 데이터에 대해 문제들의 특정 유형을 해결하도록 최적화된다. 확장의 필요성은 다양한 NoSQL과 관계형 데이터베이스로 구성된 하이브리드 아키텍처를 이끌어냈다. 하나의 크기에 맞는(one-size-fits-all) 데이터 처리 솔루션의 시대가 도래했다.

이 책은 특정 NoSQL 기술인 Apache 솔라에 대해 다룬다. 솔라는 문제들의 고유 한 유형을 위해 최적화된다. 특히 솔라는 대용량의 텍스트 중심 데이터를 검색하고 관련 적절하게 정렬된 결과를 반환하도록 최적화 된 확장성이 뛰어난 ready-to-deploy 엔터프라이즈 검색 엔진이다. 이는 아래와 같이 세분화해볼 수 있다.
* 확장 : 솔라는 분배된 작업 (인덱싱 및 쿼리 처리)이 클러스터의 여러 서버에 확장된다.
* 배포 준비 : 솔라는 오픈 소스이므로 설치 및 구성이 쉽고 사전 구성된 예제를 제공하여 시작하는 데 도움을 준다.
* 검색을 위한 최적화 : 솔라는 빠르고 복잡한 쿼리를 초당 수십 밀리 초 미만의 속도로 실행할 수 있다.
* 많은 양의 문서 : 솔라는 수백만 개의 문서가 포함 된 색인을 처리하도록 설계되었다.
* 텍스트 중심 : 솔라는 전자 메일, 웹 페이지, 이력서, PDF 문서 및 트윗이나 블로그와 같은 소셜 메시지와 같은 자연어 텍스트 검색에 최적화되어 있다.
* 적절하게 정렬 된 결과물 - 솔라는 각 문서가 사용자의 쿼리와 얼마나 관련이 있는지에 따라 순위가 정해진 순서대로 문서를 반환한다.

이 책에서는 솔라를 사용하여 확장 가능한 검색 솔루션을 설계하고 구현하는 방법을 학습할 것이다. 먼저 솔라가 지원하는 데이터 유형과 사용 사례에 대해 학습한다. 이것은 솔라가 최신 응용 프로그램 아키텍처의 큰 그림에 들어 맞다는 것을 이해시켜주고, 솔라가 해결하도록 설계된 문제들을 이해하는 데 도움이 될 것입니다.(This will help you understand where Solr fits into the big picture of modern application
architectures and which problems Solr is designed to solve.)



## 1.1 검색엔진이 필요한 이유는 무엇일까?

이 책을 보고 있기 때문에 검색 엔진이 필요한 이유에 대해 이미 알고 있다고 생각된다. 솔라를 고려하는 이유를 추측하기 보다는, 검색 엔진이 당신에게 적합한 지 판단하기 위해 당신의 데이터 및 이용 사례에 대해 답변해야하는 어려운 질문에 대해 바로 알아볼 것이다. 결국 그것은 데이터 및 사용자를 이해하고 두 가지 모두에 적합한 기술을 선택하는 것으로 이어진다. 먼저 검색 엔진이 처리할 수 있도록 최적화된 데이터의 속성을 살펴보도록 하자.



### 1.1.1 텍스트 중심 데이터 관리

최신 애플리케이션 아키텍처의 특징은 스토리지 및 처리 엔진을 데이터와 일치시키는 것이다. 프로그래머라면 알고리즘에서 데이터를 사용하는 방법에 따라 최상의 데이터 구조를 선택해야한다. 즉, 빠른 랜덤 조회가 필요할 때 링크드 리스트를 사용하지는 않을 것이다. 동일한 원칙이 검색 엔진에도 적용된다. 솔라와 같은 검색 엔진은 다음 네 가지 주요 특성을 나타내는 데이터를 처리하도록 최적화되어 있다.

1. 텍스트 중심 (Text-centric)
2. 읽기 위주 (Read-dominant)
3. 문서 지향 (Document-oriented)
4. 유연한 스키마 (Flexible schema)

다섯 번째 특성이 될만한 것으로는 대용량의 데이터를 처리해야한다는 것이다. 즉 **큰 데이터**이지만, 우리는 다른 NoSQL 기술 중에서도 검색 엔진을 특별하게 만드는 것에 초점을 맞추고 있다. 솔라가 대량의 데이터를 다룰 수 있다는 것은 두말할 필요도 없다.

솔라와 같은 검색 엔진으로 인해 효율적으로 처리되는 데이터의 네 가지 주요 특성이지만 엄격한 규칙이 아닌 대략적인 지침으로 간주해야한다. 이 특성들이 왜 검색에서 중요한지 알아보기 위해 각각을 파헤쳐 보자. 지금은 추상적(high-level) 개념들에 초점을 맞출 것이다. 그리고 나서 후반 챕터에서 **방법**에 대해 알아볼 것이다.



#### 텍스트 중심

검색 엔진에서 처리하는 데이터 유형을 나타내는 용어들 중에는 체계적이지 않은 것들이 반드시 존재한다. 우리는 인간 언어를 기반으로하는 텍스트 문서가 함축적인 구조를 가지고 있기 때문에 체계적이지 않은 것들은 다소 모호하다고 느껴진다. 텍스트를 문자 스트림으로 보는 컴퓨터의 관점에서도 체계적이지 않다고 생각할 수 있다. 문자 스트림은 구문을 추출하여 검색 엔진에서 검색 할 수 있도록 언어 별 규칙을 사용하여 구문 분석되어야한다.

우리는 텍스트 중심이 솔라가 처리하는 데이터 유형을 표현하는데 더 적절하다고 생각한다. 검색 엔진은 검색을 향상시키기 위해 함축적인 텍스트 구조를 인덱스로 추출하도록 특별히 설계 되었기 때문이다. 텍스트 중심 데이터는 문서의 텍스트가 사용자가 찾으려는 정보를 포함한다는 것을 의미한다. 물론 검색 엔진은 날짜 및 숫자와 같은 텍스트가 아닌 데이터도 지원하지만 기본적으로 자연 언어를 기반으로 텍스트 데이터를 처리하는 것이 가장 큰 장점이다.

사용자가 텍스트로 원하는 정보를 얻지 못하면 검색 엔진이 문제에 대한 최고의 솔루션이 아니기 때문에 요점(The centric part)을 파악하는 것이 중요하다. 직원이 출장비 보고서를 작성하는 어플리케이션을 생각해보자. 각 보고서에는 날짜, 비용 유형, 통화 및 금액과 같은 많은 구조화 된 데이터 필드가 있다. 또한 각 비용에는 직원이 경비에 대한 간략한 설명을 제공 할 수있는 메모 필드가 포함될 수 있다. 이것은 텍스트를 포함하지만 텍스트 중심이 아닌 데이터의 예이다. 월별 지출 보고서를 작성할 때 회계 부서에서 메모 필드를 검색해야 할 가능성은 거의 없다. 데이터에 텍스트 필드가 포함되어 있다고해서 데이터가 검색 엔진에 적합하다는 의미는 아니라는 것이다.

데이터가 텍스트 중심인지 생각해야한다. 주요 고려 사항은 데이터의 텍스트 필드에 사용자가 쿼리하려는 정보가 포함되어 있는지 여부이다. 그렇다면 해당 검색 엔진이 아마도 좋은 선택 이 될 것이다. 5 장과 6 장에서 솔라의 텍스트 분석 기능을 사용하여 텍스트의 구조를 풀어헤치는 방법을 살펴 보도록 할 것이다.



#### 읽기 위주(READ-DOMINANT)

검색 엔진이 효과적으로 처리하는 데이터의 또 다른 주요 측면은 자주 업데이트되는 것과는 대조적으로 데이터가 읽기 위주여서 효율적으로 접근 하도록 되어있다는 것이다. 솔라를 사용하면 색인된 기존 문서를 업데이트 할 수 있다. 읽기 위주라는 것은 문서가 작성 또는 업데이트되는 것보다 훨씬 자주 읽혀진다는 의미라고 생각하면 된다. 그러나 너무 많은 데이터를 작성할 수는 없다거나 새로운 데이터를 작성할 수 있는 빈도에 한계가 있을 수 있다는 것을 잊지 말아야 한다. 실제로 솔라 4의 주요 기능 중 하나는 초당 수천 개의 문서를 색인화하고 거의 즉시 검색을 할 수 있는 실시간에 근접한 (NRT) 검색이다.

읽기 위주 데이터의 핵심은 솔라에 데이터를 작성할 때 라이프타임 동안 무수히 많은 시간을 읽고 다시 읽으려는 것이다. 예를 들어, 데이터 저장 (쓰기 작업)과 달리 쿼리 실행 (읽기 작업)에 최적화 된 검색 엔진을 생각해보자. 또한 검색 엔진에서 기존 데이터를 자주 업데이트해야하는 경우에는 해당 검색 엔진이 사용자 요구에 가장 적합한 솔루션이 아닐 수도 있다. 기존 데이터에 빠른 랜덤 쓰기 기능이 필요할 때는 카산드라 (Cassandra)와 같은 다른 NoSQL 기술이 더 나은 선택 일 수 있다.
