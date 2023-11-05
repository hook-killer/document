# 📚 Search Engine Service 관련 용어 정리

- Bucket·버킷
    
    데이터 저장 공간. 연관된 객체들을 그루핑한 최상위 디렉터리
    
- Coordinate Node·코디네이트 노드
    
    외부에서 인덱싱, 검색 등의 요청을 받아 데이터 노드에 전달하고 요청 결과를 받아 반환하는 노드
    
- Data Node·데이터 노드
    
    네이버 클라우드 플랫폼의 Search Engine Service에서 제공하는 Elasticsearch 클러스터 구성 요소 중 하나로 실제 데이터가 저장되는 서버이며 코디네이트 노드로만 통신하는 노드
    
- Elasticsearch
    
    Apache Lucene을 기반으로 하는 오픈소스 RESTful 검색 및 분석 엔진으로 정형/비정형 데이터, 위치 정보, 메트릭 등 다양한 형태의 데이터를 저장하고 다양한 방법으로 검색 가능
    
- Index·색인
    
    문서의 내용을 분석해서 검색하기 용이한 형태의 데이터를 만드는 행위 혹은 그 데이터
    
- JavaScript Object Notation(JSON)·제이슨
    
    인간이 읽을 수 있는 데이터 교환용으로 설계된 경량 텍스트 기반 개방형 표준 포멧
    
- Kibana·키바나
    
    **Elasticsearch**를 위한 오픈 소스 데이터 시각화 플러그인
    
- Manager Node·매니저 노드
    
    네이버 클라우드 플랫폼의 Search Engine Service에서 제공하는 **Elasticsearch** 클러스터 구성 요소 중 하나로 **Elasticsearch** 클러스터를 관리하며 코디네이트 노드로 설정되어 데이터가 인입되는 통로로 사용하는 노드
    
- Master Node·마스터 노드
    
    클러스터에 인덱스를 생성하거나 데이터 노드에 샤드를 할당하는 등 클러스터 관리 작업을 담당하는 노드
    
- Node·노드
    
    재분배 지점이나 통신 종단점. 네트워크 기본 요소인 지역 네트워크에 연결된 컴퓨터와 그 안에 속한 장비를 통틀어 하나의 노드라고 지칭
    
- Primary Shard·프라이머리 샤드
    
    단일 데이터를 다수의 데이터베이스로 쪼개어 나눈 블록들의 구간
    
- Shard·샤드
    
    DB 클러스터에서 분할된 데이터 단위
    
- Secure Shell(SSH)·시큐어 셸(에스에스에이치)
    
    암호화 기법을 이용하여 원격 서버 로그인 후 명령 수행 시 사용하는 프로토콜