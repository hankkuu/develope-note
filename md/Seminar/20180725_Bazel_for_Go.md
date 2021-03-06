# Bazel for Go

## Bazel

* 빌드를 도와주는 시스템
* 다양한 언어 빌드 지원
* 다양한 하드웨어 플랫폼, 크로스 컴파일 지원
* 구글에서 내부 빌드툴로 사용 중
  * 오픈 소스에서는 일부 기능만 오픈
* 빌드 명세를 정확히 작성해 주어야 동작함
  * 의존되는 라이브러리 모두 명시해야함
* Skylark 룰 기반
  * python과 유사한 스크립트 언어
  * Bazel을 빌드하기 위한 언어로 사용됨
  * 참고 : https://docs.bazel.build/versions/master/skylark/language.html
* 빌드를 코드로 관리
* Docker Image 빌드 제공



## Bazel 사용의 장점

* 빠르고 정확한 빌드
  * 병렬화

    ![](images/bazel_1.png)

    * 병렬화를 위해 빌드 명세가 필요
    * 각 소스코드 파일마다 필요한 라이브러리를 알기 때문에 병렬이 가능
    * 멀티 머신 기능은 아직 작업 중

  * 캐싱

    ![](images/bazel_2.png)

    * 필요한 부분만 다시 빌드하고 나머지는 캐싱

  * 정확성

    ![](images/bazel_3.png)

    * 각 소스 파일마다 의존성이 독립적으로 관리되기 때문에 어디서 누가 빌드를 하든지 정확히 빌드
    * 순환 참조는 허용 안함.
      * A가 B를 참조하면 B는 A를 참조할 수 없음



## 참고

* 예제 : https://github.com/si-you/bazel-golang-examples
* 발표자료 : https://github.com/golangkorea/gophercon-talks