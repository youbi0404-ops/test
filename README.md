# Java Flow Explorer

레거시 Java 백엔드(Spring / Anyframe / iBATIS / Struts)의 복잡한 호출 흐름을
**URL → Controller → Service → DAO → SQL → 테이블**까지 한눈에 보여주는 로컬 전용 분석 페이지입니다.

- 파일 하나(`java-flow-explorer.html`)가 전부입니다. 설치·서버·JDK 불필요.
- 외부 통신이 전혀 없습니다(CDN·폰트·API 호출 없음). 폐쇄망 PC에서 그대로 동작합니다.
- 소스는 브라우저 메모리에서만 분석되며 어디에도 저장·전송되지 않습니다.

## 사용법

1. `java-flow-explorer.html`을 내부망 PC로 복사해 Chrome 또는 Edge로 엽니다(더블클릭).
2. **프로젝트 폴더 선택**을 누르고 프로젝트 루트(또는 `src`) 폴더를 고릅니다. 폴더를 끌어다 놓아도 됩니다.
   - 브라우저의 "파일 N개를 업로드하시겠습니까?" 확인은 로컬 읽기 허용이며 실제 전송은 없습니다.
3. 왼쪽에서 진입점(URL)·클래스·SQL·테이블을 고릅니다.

프로젝트 없이 **샘플 프로젝트로 체험** 버튼으로 먼저 살펴볼 수 있습니다.

## 화면

| 탭 | 용도 |
|---|---|
| 호출 흐름 | 선택한 메서드에서 SQL·테이블까지 내려가는 호출 트리. 텍스트로 복사해 문서에 붙여넣기 가능 |
| 시퀀스 다이어그램 | 같은 흐름을 클래스 간 실행 순서로 표시 |
| 로직 개요 | 한 메서드의 if / 반복 / try-catch 구조와 그 안의 호출만 추려서 표시 |
| 역추적 | 메서드·SQL·테이블을 "누가 쓰는지" 거슬러 올라가 영향받는 화면(URL) 목록 제공 |

## 인식 범위

- Java 소스(Java 6 문법 기준, 상위 버전도 대부분 동작), EUC-KR/MS949·UTF-8 자동 판별
- URL 매핑: `@RequestMapping`, `SimpleUrlHandlerMapping`·`BeanNameUrlHandlerMapping`(XML), Struts 1/2 설정
- 의존성 주입: Spring XML `<property ref>`(setter 주입), `@Resource(name)`, `@Qualifier`, 타입 기준(인터페이스 → 구현체)
- SQL: Anyframe Query 매핑 XML, iBATIS `sqlMap`, MyBatis `mapper`, Java 소스 내 문자열 SQL(추정)
- SQL ID가 상수나 `"a" + "b"` 결합으로 전달되는 경우도 해석

## 한계

정적 분석이므로 리플렉션, 런타임에 조립되는 SQL ID, 외부 라이브러리(jar) 내부 흐름은 추적하지 못합니다.
구현체가 여러 개인 인터페이스 호출은 후보를 모두 보여줍니다("구현체 후보" 표시).
