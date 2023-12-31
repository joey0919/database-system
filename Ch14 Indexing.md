# Ch14 Indexing

## Basic Concepts
>   * 원하는 데이터에 대한 액세스 속도를 높이는 데 사용되는 인덱싱 메커니즘
>   * Search Key - 파일에서 레코드를 조회하는 데 사용되는 속성 집합에 대한 속성이다.
>   * index file은 레코드들(index entries)로 구성된다.
>
>       ![image-68](https://github.com/joey0919/database-system/assets/87600842/4f7d6139-3778-4434-a829-6c126f8bb7f8)
>   * 인덱스 파일은 일반적으로 원본 파일보다 훨씬 작다.
>   * 두 가지 기본 종류의 인덱스:
>   * Ordered indices: search keys는 정렬된 순서로 저장된다.
>   * Hash indces: search keys는 해시함수를 사용하여 buckets에 균일하게 배포된다.

## Index Evaluation Metrics (인덱스 평가 지표)
>   * 효율적으로 지원되는 액세스 유형 예:
>       * 속성에 지정된 값이 있는 레코드
>       * 지정된 값 범위에 속하는 속성 값을 가진 레코드
>       * 액세스 시간
>       * 삽입 시간
>       * 삭제 시간
>       * 공간 오버헤드

## Ordered Indices (정렬된 인덱스)
>   * index 항목은 search key 값을 기준으로 정렬되어 저장된다.
>   * Clustering index: 순차적으로 정렬된 파일에서 검색 키가 파일의 순차적 순서를 지정하는 인덱스
>       * primary index(기본 인덱스) 라고도 함 
>       * 기본 인덱스의 search key는 일반적으로 primary key일 필요는 없음
>   * Secondary index(보조 인덱스): 검색 키가 파일의 순차적 순서와 다른 순서를 지정하는 인덱스
>       * nonclustering index라고도 함
>   * Index-sequential file: search key에 클러스터링 인덱스가 있는 search key에 따라 정렬된 순차 파일이다.

## Dense Index Files
>   * Dense index - 파일의 모든 search key 값에 대해 index record가 나타난다.
>   ![image-69](https://github.com/joey0919/database-system/assets/87600842/ee25fb63-b218-440a-9f4d-12da3cb16f2b)
>   * 밀도가 높은 dept_name sorted on dept_name
>   ![image-70](https://github.com/joey0919/database-system/assets/87600842/986bf19a-21fd-45d1-a786-249a0980a6a5)

## Sparse Index Files (희소 인덱스 파일)
>   * Sparse Index: 일부 search-key 값에 대해서만 index 레코드를 포함한다.
>       * search-key에 따라 레코드가 순차적으로 정렬되는 경우에 적용 가능
>   * search-key K값으로 레코드를 찾으려면
>       * search-key 값이 가장 큰 index 레코드 찾기 < k
>       * index 레코드가 가리키는 레코드부터 순차적으로 파일 검색
>   * 밀도가 높은 인덱스와 비교:
>       * 삽입 및 삭제에 대한 공간과 유지 관리 오버헤드가 줄어든다.
>       * 일반적으로 레코드를 찾는 데 밀집 인덱스보다 느리다.
>   * 좋은 절충안
>       * 클러스터형 인덱스의 경우: 블록의 최소 검색 키 값에 해당하는 파일의 모든 블록에 대한 인덱스 항목이 있는 희소 인덱스 이다.
>
>           ![image-71](https://github.com/joey0919/database-system/assets/87600842/519ca794-5c7a-4bd4-b890-6d7027957beb)
>       * 비클러스터형 인덱스인 경우: 밀집형 인덱스 위에 희소 인덱스(다단계 인덱스)

## Secondary Indices Example (보조 인덱스 예)
>   * 강사 급여분야에 대한 보조 지표
>   ![image-72](https://github.com/joey0919/database-system/assets/87600842/37510c19-37ac-46b9-9de9-9e64b24399c2)
>   * 인덱스 레코드는 특정 검색 키 값을 가진 모든 실제 레코드에 대한 포인터가 포함된 버킷을 가리킨다.
>   * 보조 인덱스는 조밀해야 한다.

## 클러스터링 및 비클러스터링 인덱스
>   * 인덱스는 레코드를 검색할 대 상당한 이점을 제공한다.
>   * 그러나 인덱스는 데이터베이스 수정에 오버헤드를 부과한다.
>       * 레코드가 삽입되거나 삭제되면 관계의 모든 인덱스가 업데이트되어야 한다.
>       * 레코드가 업데이트되면 업데이트된 속성의 모든 인덱스도 업데이트되어야 한다.
>   * 클러스터링 인덱스를 사용한 순차 스캔은 효율적이지만 보조(비클러스터링) 인덱스를 사용한 순차 스캔은 자기 디스크에서 비용이 많이 든다.
>       * 각 레코드 액세스는 디스크에서 새 블록을 가져올 수 있다.
>       * 자기 디스크의 각 블록을 가져오는 데 약 5~10밀리초가 필요하다.

## Multilevel Index
>   * 인덱스가 메모리에 맞지 않으면 액세스 비용이 많이 든다.
>   * 해결방법: 디스크에 보관된 인덱스를 순차 파일로 처리하고 여기에 sparse(희소) 인덱스를 구성한다.
>       * 외부 인덱스 - 기본 인덱스의 sparse(희소) 인덱스
>       * 내부 인덱스 - 기본 인덱스 파일
>   * 외부 인덱스가 너무 커서 주 메모리에 맞지 않으면 또 다른 수준의 인덱스가 생성될 수 있다.
>   * 파일에서 삽입하거나 삭제할 때 모든 수준의 인덱스를 업데이트 해야된다.
>   ![image-73](https://github.com/joey0919/database-system/assets/87600842/cbc41f53-65fc-4a6e-81c6-ef6a03bf823a)

## Index Update: Deletion
>   * 삭제된 레코드가 파일에서 특정 search-key 값을 가진 유일한 레코드인 경우 해당 검색키가 index에서 삭제된다.
>   * Single-level index 항목 삭제
>       * Dense indices: 검색 키 삭제는 파일 기록 삭제와 유사하다.
>       * Sparse indices:
>           * 검색 키에 대한 항목이 색인이 존재하는 경우 색인의 항목을 파일의 다음 검색 키 값(검색 키 순서)으로 대체하여 삭제된다.
>           * 다음 검색 키 값이 이미 index 항목이 있으면 항목이 바뀌는 대신 삭제된다.

## Index Update: Insertion
>   * Single-level index insertion:
>       * 삽입할 레코드의 검색키 값을 이용하여 조회한다.
>       * Dense indices - serch-key 값이 index에 나타나지 않으면 삽입한다.
>           * index는 순차 파일로 유지된다.
>           * 새 항목을 위한 공간을 만들어야 하며 오버플로 블록이 필요할 수 있다.
>       * Sparse indices - index가 파일의 각 블록에 대한 항목을 저장하는 경우 새 블록이 생성되지 않는 한 index를 변경할 필요가 없다.
>   * Multilevel insertion and deletion: 알고리즘은 단일 수준 알고리즘의 간단한 확장이다.

## Indices on Multiple Keys
>   * Composite search key(복합 검색키)
>   * E.g., index on instructor relation on attributes (name, ID)
>   * 값은 사전순으로 정렬된다.
>       * E.g. (John, 12121) < (John, 13514) and (John, 13514) < (Peter, 11223)
>   * 다음 항목에 대해 쿼리할 수 있다. name, or (name, ID)

## B+-Tree Index Files
>   * indexed-sequential 파일의 단점
>       * 파일이 커지면 오버플로 블록이 많이 생성되므로 성능이 저하된다.
>       * 전체 파일의 주기적인 재구성이 필요하다.
>   * B+트리의 index files의 장점
>       * 삽입 및 삭제 시 작은 로컬 변경 사항으로 자동으로 재구성 된다.
>       * 성능 유지를 위해 전체 파일을 재구성할 필요가 없다.
>   * 단점:
>       * 추가 삽입 및 삭제 오버헤드, 공간 오버헤드
>   * B+트리의 장점이 단점보다 중요하다
>   ![image-74](https://github.com/joey0919/database-system/assets/87600842/94e81c6e-16cc-4fc2-88db-2459e835db44)
>   * 루트에서 리프까지의 모든 경로의 길이는 동일하다.
>   * 루프나 리프가 아닌 각 노드는 [n/2] ~ n
>   * 리프 노드는 [(n-1)/2] 와 n-1 사이 값
>   * 특수한 상황들:
>       * 루트가 리프가 아닌 경우에는 최소한 2개의 자식이 있다.
>       * 루트가 리프인 경우(즉, 트리에 다른 노드가 없는 경우) 0 ~ (N-1)

## B+ 트리 노드 예제
>   ![image-75](https://github.com/joey0919/database-system/assets/87600842/7235b099-ed78-4b0e-aa40-e31e96bd380b)
>   * 리프 노드에는 3~5개의 값이 있어야 한다 ((n–1)/2 and n –1, with n = 6).
>   * 루트 이외의 리프가 아닌 노드에는 3~6개의 하위 노드가 있어야 한다.  (n/2 and n with n =6).
>   * 루트에는 자식이 2명이상 있어야 한다.

## Other Issues in Indexing
>   * 기록 재배치 및 보조 색인
>       * 레코드가 이동하면 레코드 포인터를 저장하는 모든 보조 인덱스를 업데이트 해야한다.
>       * B+-트리 파일 조직의 노드 분할은 매우 비싸진다.
>       * 해결책: 보조 인덱스의 레코드 포인터 대신 B+트리 검색키를 사용한다.
>           * B+트리 파일 구성 검색 키가 고유하지 않은 경우 record-id 추가
>           * 기록을 찾기 위한 파일 구성의 추가 탐색
>               * 쿼리 비용은 높지만 노드 분할은 저렴

## Indexing Strings
>   * 가변 길이 문자열을 키로 사용
>       * Variable fanout
>       * 포인터 개수가 아닌 공간 활용도를 분할 기준으로 사용
>   * 접두사 압축
>       * 내부 노드의 키 값은 전체 키의 접두사가 될 수 있다.
>           * 키 값으로 구분된 하위 트리의 항목을 구별할 수 있도록 충분한 문자를 유지
>               * 예를 들어 "Silas"와 "Silberschatz"는 "Silb"로 구분될 수 있다.
>       * 공통 접두사를 공유하여 리프 노드의 키를 압축할 수 있다.

## Indexing on Flash
>   * 플래시에서의 임의 I/O 비용이 훨씬 저렴하다.
>   * 많은 비용이 드는 삭제
>   * 대량 로딩은 페이지 삭제를 최소화하므로 유용

## Indexing in Main Memory
>   * 메모리의 무작위 접근
>       * 디스크/플래시보다 훨씬 저렴
>       * 캐시 읽기에 비해 많은 비용
>       * 캐시를 최대한 활용하는 것이 바람직한 데이터 구조
>       * 큰 B+트리 노드에서 이진 탐색시 캐시 누락이 많이 발생
>   * B+트리 캐시미스를 줄이기 위해 캐시 라인에 맞는 작은 노드를 가진 트리가 바람직함
>   * 큰 노드 크기를 사용하여 디스크 액세스를 최적화하되 배열을 사용하는 대신 작은 노드 크기의 트리를 사용하여 노드 내의 데이터를 구조화 함

## Static Hashing (정적 해싱)
>   * bucket은 하나 이상의 항목을 포함하는 저장소 단위이다.(일반적으로 디스크 블록)
>       * 다음을 사용하여 search-key 값에서 bucket을 얻는다 -> hash function
>   * 해시 함수 h는 모든 검색 키 값 K의 집합에서 다음과 같은 함수이다. 모든 버킷 주소들의 집합 B.
>   * 해시 함수는 액세스, 삽입, 삭제 및 입력 항목을 찾는 데 사용된다.
>   * 검색 키 값이 다른 항목은 동일한 항목에 매핑될 수 있다. 따라서 전체 버킷을 찾기 위해 순차적으로 검색해야 합니다
>   * 해시 인덱스에서 버킷은 레코드에 대한 포인터와 함께 항목을 저장한다.
>   * Hash file-organization 버킷에 레코드 저장

## Handling of Bucket Overflows
>   * 버킷 오버플로는 다음과 같은 이유로 발생할 수 있다.
>       * 버킷이 부족함
>       * records의 분포가 왜곡됨. 이는 다음 두 가지 이유로 인해 발생
>           * 여러 레코드에 동일한 검색 키 값이 있다.
>           * 선택한 해시 함수는 키 값의 균일하지 않은 분포를 생성
>   * Overflow chaining - 특정 버킷의 오버플로 버킷은 연결된 목록으로 함께 연결된다.
>   * closed addressing(폐쇄 주소 지정)
>       * open addressing은 오버플로 버킷을 사용하지 않으며 데이터베이스에 적합하지 않다.


## Deficientcies of Static Hashing (정적 해싱 결함)
>   * 정적 해싱에서 함수 h는 검색 키 값을 버킷 주소 고정 집합 B에 매핑합니다. 데이터베이스는 시간에 따라 증가하거나 감소한다.
>       * 버킷의 초기 수가 너무 적고 파일이 증가하는 경우 너무 많은 오버플로로 인해 성능이 저하
>       * 예상되는 성장을 위해 공간이 할당된 경우 처음에는 상당한 양의 공간이 낭비된다.
>       * 데이터베이스가 축소되면 다시 공간이 낭비됨
>   * 해결책: 새로운 해시 함수를 사용하여 파일을 주기적으로 재구성
>       * 비용이 많이 들고 정상적인 운영을 방해
>   * 더 나은 솔루션: 버킷 수를 동적으로 수정하도록 허용한다.

## 동적 해싱
>   * Periodic rehashing (주기적 재해싱)
>       * 해시 테이블의 항목 수가 해시 테이블 크기의 1.5배가 된다면,
>           * 이전 해시 테이블 크기의 2배 크기의 새 해시 테이블을 만든다.
>           * 모든 항목을 새 테이블로 다시 해시한다.
>   * Linear Hashing
>       * 증분 방식으로 재해싱 수행
>   * Extendable Hashing (확장 가능한 해싱)
>       * 여러 해시 값으로 공유되는 버킷을 사용하여 디스크 기반 해싱에 맞게 조정
>       * 버킷 수를 두 배로 늘리지 않고 해시 테이블의 항목 수를 두 배로 늘림

## Comparison of Ordered Indexing and Hashing (순서형 인덱싱과 해싱의 비교)
>   * 주기적인 재구성 비용
>   * 삽입 및 삭제의 상대적 빈도
>   * 최악의 액세스 시간을 희생하면서 평균 액세스 시간을 최적화 하는 것이 바람직할까?
>   * In practice:
>       * PostgreSQL은 해시 인덱스를 지원하지만 성능 저하로 인해 사용을 권장하지 않음
>       * Oracle은 정적 해시 구성을 지원하지만 해시 인덱스는 지원하지 않음
>       * SQLServer는 B+트리만 지원함

## Index Definition in SQL
>   * create index (index-name) on (relation-name)
>   * drop index (index-name)
>   * 대부분의 데이터베이스 시스템은 인덱스 유형 지정 및 클러스터링을 허용
