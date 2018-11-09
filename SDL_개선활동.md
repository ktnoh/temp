- [SDL과제정밀분석 + CVE 분석](#sdl%EA%B3%BC%EC%A0%9C%EC%A0%95%EB%B0%80%EB%B6%84%EC%84%9D--cve-%EB%B6%84%EC%84%9D)
    - [기대효과](#%EA%B8%B0%EB%8C%80%ED%9A%A8%EA%B3%BC)
    - [상세 운영 계획](#%EC%83%81%EC%84%B8-%EC%9A%B4%EC%98%81-%EA%B3%84%ED%9A%8D)
        - [과제 선정 대상](#%EA%B3%BC%EC%A0%9C-%EC%84%A0%EC%A0%95-%EB%8C%80%EC%83%81)
        - [수행 계획](#%EC%88%98%ED%96%89-%EA%B3%84%ED%9A%8D)
            - [CVE 분석](#cve-%EB%B6%84%EC%84%9D)
            - [SDL 지난 과제 분석](#sdl-%EC%A7%80%EB%82%9C-%EA%B3%BC%EC%A0%9C-%EB%B6%84%EC%84%9D)

# SDL과제정밀분석 + CVE 분석
## 기대효과

* 연속성 있는 과제의 ATTACK SURFACE 선확보
* 과제의 보안적 측면에 대한 이해도 제고
* 당사 관련 CVE에 대한 HISTORY 축적
* 개발자에게 제공할 보안검증 산출물 모범 사례 확보

## 상세 운영 계획

### 과제 선정 대상

* 보안검증에 연속성 있는 과제
* EX) SES, Tizen, SMP, Star, Dover

### 수행 계획

#### CVE 분석
* 2주 단위로 반복
  * 1주 차: 인당 cve 하나씩 스터디 후 공유 (개인)
  * 2주 차: 공유된 cve 중 하나를 선정해 재연 (deep dive) (공동)
* 결과를 사내 github wiki에 markdown 형식으로 공유

#### SDL 지난 과제 분석
* 기 검증 결과 회고
    * 과제에 대한 히스토리 정리 (timeline)
    * 기 이슈 리뷰
* Threat modeling 다시 작성
* Attack surface 분석 후 기록