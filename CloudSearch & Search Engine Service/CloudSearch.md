# 🔎 Cloud Search

> :warning: 기술을 사용할지 고민하다 다른 기술을 사용하기로 하여 정리가 미흡합니다. 양해 바랍니다.

[Cloud Search 관련 용어 정리](./용어정리/Cloud%20Search%20관련%20용어정리.md)

[Cloud Search 개요](https://guide.ncloud-docs.com/docs/cloudsearch-overview)

웹 사이트에 필요한 검색 기능을 쉽게 구현하게 하는 클라우드 기반의 플랫폼.

별도의 인프라 구축 없이 간편하게 검색 엔진 만들기 가능.

DB 데이터의 업로드 과정 >>>

1. DB 데이터 업로드 요청 시 입력한 정보로 서버에서 데이터 질의
2. DB 서버에서 데이터를 받아 Cloud Search 문서(JSON)로 변환
3. 변환된 문서는 Cloud Search에 순차적으로 업로드되어 인덱싱

### Domain

- 도메인 생성하여 DB 연결이 가능해짐.
- 이미 만들어놓은 DB Server에 연결이 가능하고, 이 DB에서 내가 Indexing 방식, 검색 대상 문서를 설정할 수 있음.

![Alt text](./CloudSearch%20Images/image.png)

![Alt text](./CloudSearch%20Images/image-1.png)

- 색인용 컨테이너?? 검색용 컨테이너??
    
    index를 가지는 컨테이너와 index로 찾아나갈 수 있는 데이터들이 있는 검색용 컨테이너가 존재
    
- 도메인의 섹션과 색인은 검색할 문서의 형식과 검색 목적에 맞게 정의해야함.
- 이후 검색 시 결과를 나타내는 데에 이용되는 가중치를 설정 가능
    - 섹션 가중치 : 해당 섹션 정보의 중요도를 나타냄
    - 문서 가중치 : 문서 가중치 함수에 따라 섹션 가중치를 이용하여 문서 가중치를 계산하며 별도 설정이 없으면 문서 가중치가 높은 순서대로 검색 결과 정렬
- StopWord(불용어), 동의어 설정 가능

### Monitoring

검색 추이에 대한 모니터링 가능.

네이버 콘솔에서 GUI로 제공

### Query

[검색 쿼리](https://guide.ncloud-docs.com/docs/cloudsearch-searchquery)

- 검색 요청 방법
    - 요청 바디에 입력
    - URI에 입력

### Ranking

[랭킹 수식](https://guide.ncloud-docs.com/docs/cloudsearch-ranking)

정렬 방식 별도 설정 없을 시 문서 가중치가 높은 순서대로 정렬.

검색 결과를 정렬하는 기준인 랭킹 점수를 계산하는 식이 **랭킹 수식**