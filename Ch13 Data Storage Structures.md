# Ch13 Data Storage Structures

## File Organization
>   * 데이터베이스는 파일의 집합체로 저장됩니다. 레코드는 필드의 연속입니다
>   * 하나의 접근 방식
>       * 레코드 크기가 고정되어 있다고 가정
>       * 각 파일에는 특정 유형의 레코드만 있다.
>       * 서로 다른 관계에 서로 다른 파일이 사용된다.
>   * 레코드가 디스크 블록보다 작다고 가정한다.

## Fixed-Length Records (고정길이 레코드)
>   * 간단한 접근 방식
>       * 레코드 i는 n*(i-1) 부터 시작된다. n은 레코드의 크기이다.
>       * 레코드 접근은 간단하지만 블록을 넘을 수 있다
>       * 수정: 레코드가 블록 경계를 넘는 것을 허용하지 않는다.
>       * ![image-52](https://github.com/joey0919/database-system/assets/87600842/f97b356b-2916-46b1-b79e-8d9dae2e8d43)
>   * i 레코드 삭제시 대안
>       * move records i + 1, . . ., n to i, . . . , n – 1 (Record 3 deleted)
>       * ![image-53](https://github.com/joey0919/database-system/assets/87600842/f225904f-bf0c-4d68-b9ab-68aa7e160b7b)
>       * move record n to i (Record 3 deleted and replaced by record 11)
>       * ![image-54](https://github.com/joey0919/database-system/assets/87600842/51632df7-ffa1-48ac-b4e8-666748613975)
>       * 레코드를 이동하지 않고 free list의 모든 free레코드를 연결한다. (do not move records, but link all free records on a free list)
>       * ![image-55](https://github.com/joey0919/database-system/assets/87600842/3f6d9cf2-0829-4df8-9713-2f460f3d5150)

## Variable-Length Records (가변길이 레코드)
>   * 가변 길이 레코드는 데이터베이스 시스템에서 여러 가지 방법으로 발생한다.
>       * 여러 레코드 유형을 파일에 저장
>       * 문자열 (varchar)
>       * 반복 필드를 허용하는 레코드 유형(일부 이전 데이터 모델에서 사용됨)
>   * 속성은 순서대로 저장된다.
>   * 고정된 크기(offset, length)로 표현되는 가변 길이 속성. 모든 고정 길이 속성 뒤에 실제 데이터가 저장됨.
>   * 널값 비트맵으로 표현되는 널값
> ![image-56](https://github.com/joey0919/database-system/assets/87600842/2577f7a2-2263-488b-8f09-9b294e5e54bd)

## 가변 길이 레코드: 슬롯 페이지 구조
>   ![image-57](https://github.com/joey0919/database-system/assets/87600842/b9e7297c-3691-4994-b2f3-0e8433424a2f)
>   * 슬롯 페이지 헤더에는 다음이 포함된다.
>       * 레코드 항목 수
>       * 블록의 여유 공간 끝
>       * 각 레코드의 위치와 크기
>   * 레코드는 페이지 내에서 이동하여 기록 사이에 빈 공간 없이 연속적으로 유지할 수 있다. 헤더 항목을 업데이트 해야된다.
>   * 포인터는 레코드를 직접 가리켜서는 안된다. 대신 헤더의 레코드 항목을 가리켜야 한다.


## 대형 객체 저장
>   * 예: blob/clob 유형
>   * 레코드는 페이지보다 작아야 한다.
>   * 대안:
>       * 파일 시스템에 파일로 저장
>       * 데이터베이스에서 관리하는 파일로 저장
>       * 조각으로 나누고 별도의 관계에 있는 여러 튜플에 저장한다.
>           * PostgreSQL TOAST

## 파일의 기록 구성
>   * Heap - 레코드는 파일 내 공간이 있는 어느 곳에나 배치될 수 있다.
>   * Sequential - 각 레코드의 검색 키 값을 기준으로 레코드를 순차적으로 저장한다.
>       * Motivation: 관련 레코드를 동일한 블록에 저장하여 I/O를 최소화 시킨다.
>   * B+-tree 파일 구성
>       * 삽입/삭제에도 불구하고 정렬된 스토리지
>   * Hashing - 검색 키에 대해 계산된 해시 함수; 결과는 레코드가 배치되어야 하는 파일의 블록을 지정한다.

## Heap 파일 구성
>   * 여유 공간이 있는 파일에 어느 곳에나 레코드를 배치할 수 있다.
>   * 레코드는 일반적으로 할당되면 이동하지 않는다.
>   * 파일 내 여유 공간을 효율적으로 찾는 것이 중요.
>   * Free-space map
>       * 블록당 항목이 1개인 배열이다. 각 항목은 몇 비트에서 바이트이며 사용 가능한 블록의 일부를 기록한다.
>       * 아래 예에서 블록당 3비트, 값을 8로 나눈 값은 사용 가능한 블록의 비율을 나타낸다.
>           * ![image-58](https://github.com/joey0919/database-system/assets/87600842/50005536-456b-4572-bd94-b9a77617b038)
>       * 2단계 second-level free-space map을 가질 수 있다.
>       * 아래 예에서 각 항목은 첫 번째 수준 자유 공간 맵의 최대 4개 항목을 저장한다.
>           * ![image-59](https://github.com/joey0919/database-system/assets/87600842/06caa67a-73e5-443a-9283-2e9094a1fe91)
>   * 주기적으로 디스크에 Free-space map이 기록된다. 일부 항목에 대해 잘못된(이전)값이 있어도 괜찮다.(감지 및 수정 예정)

## Sequential File Organization
>   * 전체 파일을 순차적으로 처리해야 하는 애플리케이션에 적합
>   * 파일의 레코드는 search-key 순서로 정렬된다.
>       * ![image-60](https://github.com/joey0919/database-system/assets/87600842/9afeb3f0-423a-4030-b3ee-6be595ded837)
>   * 삭제 - 포인터 체인 사용
>   * 삽입 - 레코드가 삽입될 위치를 찾는다.
>       * 여유 공간이 있으면 그곳에 삽입한다.
>       * 여유 공간이 없으면 레코드를 overflow block에 삽입한다.
>       * 두 경우 모두 포인터 체인을 업데이트 해야한다.
>   * 순차적 순서를 복원하기 위해 수시로 파일을 재구성 해야한다.

## Multitable Clustering File Organization
>   * 다음을 사용하여 여러 관계를 하나의 파일에 저장한다. Multitable Clustering File Organization
>   ![image-62](https://github.com/joey0919/database-system/assets/87600842/7b2498c0-28e0-42bc-bbd9-c5f3a9270f6c)
>   * 다음과 관련된 쿼리에 적합하다. department ⨝ instructor (단일 부서 및 해당 강사와 관련된 쿼리의 경우)
>   * only department와 관련된 쿼리에는 적합하지 않다. 
>   * 결과적으로 가변 크기 레코드가 생성된다.
>   * 특정 관계의 레코드를 연결하기 위해 포인터 체인을 추가할 수 있다.

## Partitioning
>   * 테이블 파티셔닝: 관계의 레코드는 별도로 저장되는 더 작은 관계로 분할될 수 있다.
>   * 예: transaction relation은 다음과 같이 나눌 수 있다.
>   transaction_2018, transaction_2019 등
>   * 다음에 작성된 쿼리 transaction은 모든 파티션의 레코드에 엑세스 해야된다.
>       * 쿼리에 다음과 같은 선택 항목이 없으면연도=2019(이 경우 하나의 파티션만 필요함)
>   * partitioning
>       * 여유 공간 관리 등 일부 작업 비용 절감
>       * 다양한 파티션을 다양한 저장 장치에 저장할 수 있다.
>           * 예: transaction 현재 연도의 partition은 SSD에, 이전 연도의 파티션은 Magnetic disk에 저장

## Data Dictionary Storage
>   * 데이터에 대한 대이터 이다.
>   * 관계에 대한 정보
>       * 관계의 이름
>       * 각 관계의 속성 이름, 유형 및 길이
>       * 뷰의 이름과 정의
>       * 무결성 제약
>   * 비밀번호를 포함한 사용자 및 계정 정보
>   * 통계 및 설명 데이터
>       * 각 릴레이션의 튜플 수
>   * 물리적 파일 구성 정보
>       * 관계가 저장되는 방식(순차/해시/...)
>       * 관계의 물리적 우치
>   * 인덱스에 대한 정보

## Relational Representation of System Metadata (시스템 메타데이터의 관계형 표현)
>   ![image-63](https://github.com/joey0919/database-system/assets/87600842/43863b6e-3455-4680-a3d9-1b946dafc046)

## Storage Access
>   * 블록은 스토리지 할당과 데이터 전송의 단위이다.
>   * 데이터베이스 시스템은 디스크와 메모리 사이의 블록 전송 수를 최소화 하려고 한다. 메인 메모리에 가능한 많은 블록을 유지함으로써 디스크 엑세스 횟수를 줄일 수 있다.
>   * Buffer - 디스크 블록의 복사본을 저장하는 데 사용할 수 있는 주 메모리의 일부이다.
>   * Buffer manager - 메인 메모리에 버퍼 공간을 할당하는 역할을 담당하는 하위 시스템이다.

## Buffer Manager
>   * 프로그램은 디스크에서 블록이 필요할 때 버퍼 관리자를 호출한다.
>       * 블록이 이미 버퍼에 있으면 버퍼 관리자는 주 메모리에 있는 블록의 주소를 반환한다.
>       * 블록이 버퍼에 없으면 버퍼 관리자는
>           * 블록에 대한 버퍼 공간을 할당한다.
>               * 필요한 경우 새 블록을 위한 공간을 만들기 위해 다른 블록을 교체한다.
>               * 디스크에 쓰거나 디스크에서 가져온 가장 최근 시간 이후 수정된 경우에만 디스크에 다시 쓰여지는 교체된 블록이다.
>   * 디스크에서 버퍼로 블록을 읽고, 메인 메모리에 있는 블록의 주소를 요청자에게 반환한다.

>   * Buffer replacement strategy
>   * Pinned block: 디스크에 다시 쓸 수 없는 메모리 블록
>       * Pin 블록에서 데이터를 읽거나 쓰기 전에 수행됨
>       * Unpin 읽기/쓰기가 완료되면 완료된다.
>       * 여러 개의 동시 pin/unpin 작업 가능
>           * pin 수를 유지한다. 버퍼 블록은 핀 수 = 0 인 경우에만 제거될 수 있다.
>   * Shared and exclusive locks on buffer
>       * 페이지 내용이 이동/재구성될 때 동시 작업이 페이지 내용을 읽는 것을 방지하고 하나만 보장하기 위해 필요하다. 한 번에 이동/재정리
>       * Readers는  shared lock을 받고, 블록 업데이트에는 exclusive lock이 필요하다.
>   * Locking rules:
>       * 한 번에 하나의 exclusive lock을 얻을 수 있다.
>       * Shared lock은 exclusive lock과 동시에 사용할 수 없다.
>       * Multiple processes는 동시에 shared lock이 부여될 수 있다.

## Buffer-Replacement Policies
>   * 대부분의 운영 체제는 블록을 대체한다. 가장 최근에 사용됨(LRU)
>       * LRU의 기본 아이디어 - 미래 참조의 예측 변수로 블록 참조의 과거 패턴을 사용한다.
>       * LRU는 일부 쿼리에 좋지 않을 수 있다.
>   * 쿼리에는 잘 정의된 엑세스 패턴(Sequential scans)이 있으며 데이터베이스 시스템은 사용자 쿼리의 정보를 사용하여 향후 참조를 예측할 수 있다.
>   * 쿼리 최적화 프로그램에서 제공하는 대체 전략에 대한 힌트와 혼합 전략이 바람직하다.
>   * LRU에 대한 잘못된 액세스 패턴의 예: 중첩 루프로 2개의 관계 r과 s의 조인을 계산할 때
>   ![image-64](https://github.com/joey0919/database-system/assets/87600842/a1f4fde8-ef15-45da-bad9-aaa0d8eaf445)
>   * Toss-immediate strategy - 해당 블록의 마지막 튜플이 처리되자마자 해당 블록이 차지한 공간을 해제한다.
>   * 가장 최근에 사용된(MRU)전략 - 시스템은 현재 처리 중인 블록을 고정해야 한다. 해당 블록의 마지막 튜플이 처리된 후 블록의 고정이 해제되고 가장 최근에 사용된 블록이 된다.
>   * 버퍼 관리자는 요청이 특정 관계를 참조할 확률에 관한 통계 정보를 사용할 수 있다.
>       * 예를 들어, 데이터 사전에 자주 엑세스 된다. Heuristic: 데이터 사전 블록을 주 메모리 버퍼에 유지한다.
>   운영 체제 또는 버퍼 관리자가 쓰기 순서를 변경할 수 있다.
>       * 디스크의 데이터 구조가 손상될 수 있다.
>           * 예: 디스크에 누락된 블록이 있는 링크된 블록 목록
>           * 파일 시스템은 이러한 상황을 감지하기 위해 일관성 검사를 수행한다.
>       * 쓰기 순서를 주의 깊게 정하면 이러한 많은 문제를 피할 수 있다.

## Optimization of Disk Block Access (디스크 블록 엑세스 최적화)
>   * Nonvolatile write buffers(비휘발성 쓰기 버퍼)는 비휘발성 RAM 또는 플래시 버퍼에 블록을 즉시 기록하여 디스크 쓰기 속도를 높인다.
>   * Log disk: 블록 업데이트의 순차적 로그를 작성하는 데 사용되는 디스크
>       * 비휘발성 RAM과 동일하게 사용됨
>   * Journaling file systems(저널링 파일 시스템): NV-RAM 또는 로그 디스크에 순서대로 데이터 쓰기
>       * 저널링 없이 재정렬: 파일 시스템 데이터 손상 위험

## Column-Oriented Storage
>   * 관계의 각 속성을 별도로 저장
>   ![image-65](https://github.com/joey0919/database-system/assets/87600842/e04fb670-0250-455b-abde-5ccddaaabb19)

## Columnar Representation
>   * Benefits:
>       * 일부 속성에만 액세스하는 경우 IO 감소
>       * 향상된 CPU 캐시 성능
>       * 향상된 압축
>       * 최신 CPU아키텍처에서 Vector processing 
>   * Drawbacks:
>       * 열 표현에서 튜플 재구성 비용
>       * 튜플 삭제 및 업데이트 비용
>       * 압축 비용
>   * 행 기반 표현보다 의사 결정 지원에 더 효율적이 것으로 밝혀진 열 기반 표현
>   * 트랜잭션 처리에 선호되는 전통적인 행 중심 표현
>   * 일부 데이터베이스는 두가지 표현을 모두 지원한다.
>       * Called hybrid row/column stores

## Columnar File Representation
>   ![image-66](https://github.com/joey0919/database-system/assets/87600842/c75db81c-920d-4f94-a70b-11edfcdb5122)

## 메인 메모리 데이터베이스의 저장 조직
>   * 버퍼 관리자 없이 메모리에 직접 레코드를 저장할 수 있다.
>   * Column-oriented storage는 의사 결정을 위해 in-memory로 사용될 수 있다.
>       * 압축 감소 메모리 요구사항
>   ![image-67](https://github.com/joey0919/database-system/assets/87600842/3ff25096-061d-47d7-843e-e36730b822f1)
