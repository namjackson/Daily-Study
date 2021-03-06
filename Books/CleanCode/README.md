# clean-code

Clean Code 책 스터디

## 목차

[1장 깨끗한 코드](Chapter01.md)
* 코드가 존재하리라
* 나쁜 코드
* 나쁜 코드로 치르는 대가
* 우리들 생각
* 우리는 저자다
* 보이스카우트 규칙
* 프리퀄과 원칙

[2장 의미 있는 이름](Chapter02.md)
* 들어가면서
* 의도를 분명히 밝혀라
* 그릇된 정보를 피하라
* 의미 있게 구분하라
* 발음하기 쉬운 이름을 사용하라
* 검색하기 쉬운 이름을 사용하라
* 인코딩을 피하라
* 자신의 기억력을 자랑하지 마라
* 클래스 이름
* 메서드 이름
* 기발한 이름은 피하라
* 한 개념에 한 단어를 사용하라
* 말장난을 하지 마라
* 해법 영역에서 가져온 이름을 사용하라
* 문제 영역에서 가져온 이름을 사용하라
* 의미 있는 맥락을 추가하라
* 불필요한 맥락을 없애라
* 마치면서

3장 함수
* 작게 만들어라!
* 한 가지만 해라!
* 함수 당 추상화 수준은 하나로!
* Switch 문
* 서술적인 이름을 사용하라!
* 함수 인수
* 부수 효과를 일으키지 마라!
* 명령과 조회를 분리하라!
* 오류 코드보다 예외를 사용하라!
* 반복하지 마라!
* 구조적 프로그래밍
* 함수를 어떻게 짜죠?

4장 주석
* 주석은 나쁜 코드를 보완하지 못한다
* 코드로 의도를 표현하라!
* 좋은 주석
* 나쁜 주석

[5장 형식 맞추기](Chapter05.md)
* 형식을 맞추는 목적
* 적절한 행 길이를 유지하라
* 가로 형식 맞추기
* 가짜 범위
* 팀 규칙
* 밥 아저씨의 형식 규칙

[6장 객체와 자료 구조](Chapter06.md)
* 자료 추상화
* 자료/객체 비대칭
* 디미터 법칙
* 자료 전달 객체

7장 오류 처리
* 오류 코드보다 예외를 사용하라
* Try-Catch-Finally 문부터 작성하라
* 미확인unchecked 예외를 사용하라
* 예외에 의미를 제공하라
* 호출자를 고려해 예외 클래스를 정의하라
* 정상 흐름을 정의하라
* null을 반환하지 마라
* null을 전달하지 마라

8장 경계
* 외부 코드 사용하기
* 경계 살피고 익히기
* log4j 익히기
* 학습 테스트는 공짜 이상이다
* 아직 존재하지 않는 코드를 사용하기
* 깨끗한 경계

9장 단위 테스트
* TDD 법칙 세 가지
* 깨끗한 테스트 코드 유지하기
* 깨끗한 테스트 코드
* 테스트 당 assert 하나
* F.I.R.S.T.

10장 클래스
* 클래스 체계
* 클래스는 작아야 한다!
* 변경하기 쉬운 클래스

11장 시스템
* 도시를 세운다면?
* 시스템 제작과 시스템 사용을 분리하라
* 확장
* 자바 프록시
* 순수 자바 AOP 프레임워크
* AspectJ 관점
* 테스트 주도 시스템 아키텍처 구축
* 의사 결정을 최적화하라
* 명백한 가치가 있을 때 표준을 현명하게 사용하라
* 시스템은 도메인 특화 언어가 필요하다

12장 창발성(創發性)
* 창발적 설계로 깔끔한 코드를 구현하자
* 단순한 설계 규칙 1: 모든 테스트를 실행하라
* 단순한 설계 규칙 2~4: 리팩터링
* 중복을 없애라
* 표현하라
* 클래스와 메서드 수를 최소로 줄여라

13장 동시성
* 동시성이 필요한 이유?
* 난관
* 동시성 방어 원칙
* 라이브러리를 이해하라
* 실행 모델을 이해하라
* 동기화하는 메서드 사이에 존재하는 의존성을 이해하라
* 동기화하는 부분을 작게 만들어라
* 올바른 종료 코드는 구현하기 어렵다
* 스레드 코드 테스트하기
* 구현하라

14장 점진적인 개선
* Args 구현
* Args: 1차 초안
* String 인수

15장 JUnit 들여다보기
* JUnit 프레임워크

16장 SerialDate 리팩터링
* 첫째, 돌려보자
* 둘째, 고쳐보자

17장 냄새와 휴리스틱
* 주석
* 환경
* 함수
* 일반
* 자바
* 이름
* 테스트
