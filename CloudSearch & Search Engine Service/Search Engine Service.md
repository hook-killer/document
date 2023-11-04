# Cloud Search vs. Search Engine Service

# Search Engine Service

[Search Engine Service 관련 용어 정리](https://www.notion.so/Search-Engine-Service-efa202612e154263be9d516cb32af1ad?pvs=21)

공식문서 : [Search Engine Service 개요](https://guide.ncloud-docs.com/docs/ko/ses-overview)

ELK → ElasticSearch + Logstash + Kibana : [[이렇게 사용하세요!] 네이버 클라우드 플랫폼을 활용해 ELK(Elasticsearch+ Logstash +Kibana) 스택 구축하기](https://medium.com/naver-cloud-platform/이렇게-사용하세요-네이버-클라우드-플랫폼을-활용해-elk-elasticsearch-logstash-kibana-스택-구축하기-63652ebd5f32)

![Alt text](./Search%20Engine%20Service%20Images/image.png)

![Alt text](./Search%20Engine%20Service%20Images/image-1.png)

![Alt text](./Search%20Engine%20Service%20Images/image-2.png)

### Elastic Search

[[Elastic Search] 기본 개념과 특징(장단점)](https://jaemunbro.medium.com/elastic-search-%EA%B8%B0%EC%B4%88-%EC%8A%A4%ED%84%B0%EB%94%94-ff01870094f0)

[[Elasticsearch] 기본 개념잡기](https://victorydntmd.tistory.com/308)

[Elastic 가이드 북](https://esbook.kimjmin.net/)

[Set up Elasticsearch | Elasticsearch Guide [8.10] | Elastic](https://www.elastic.co/guide/en/elasticsearch/reference/current/setup.html)

[Logstash를 활용한 Cloud Database(MySQL) 연동](https://guide.ncloud-docs.com/docs/ses-example02)

- 매니저노드 : 클러스터 관리. 데이터 인입되는 통로, 코디네이트 노드 역할 & Kibana 설치될 예정 → Private / Public 모두 가능할듯. 어차피 데이터 노드가 private이니까 public 설정해도 상관없을듯? 아니면 Private 설정해놓고 LB로 잡아도 상관없을듯
    - **Port : 9200**
- 마스터노드 : 데이터노드 관리하는 노드…? → Private Subnet으로 구성해야함(선택적. 필수는 아님!)
- 데이터노드 : 데이터가 저장되는 서버. → Private Subnet으로 구성

> **Cluster ~ LoadBalancer 설정**
> 
> 
> ![Alt text](./Search%20Engine%20Service%20Images/image-3.png)
> 
> - 서버와 같은 VPC 안에 Search Engine Service를 집어넣어야 하기 때문에..
> - 그리고 TCP를 사용해야 하므로 NetworkLoadBalancer를 구축한다
> 
> ![Alt text](./Search%20Engine%20Service%20Images/image-4.png)
> 
> - ..로드밸런서를 추가해주어야 한다. 타겟 그룹은 아래와 같다. [elastic.sh-bong.site](http://elastic.sh-bong.site)로 들어가면 Kibana GUI가 나타난다.
> 
> ![Alt text](./Search%20Engine%20Service%20Images/image-5.png)
> 
> ![Alt text](./Search%20Engine%20Service%20Images/image-6.png)
> 
> - 매니저 노드에 SSH로 접속할 수 있는 환경을 만들기 위해 [링크](https://guide.ncloud-docs.com/docs/ses-ssh-vpc)에 있는 과정을 수행한다.
>     - 이 때 매니저 노드가 두개인데 어떤 로드밸런싱 알고리즘을 타는가? 가 궁금했다.
>         - L4 Hash 알고리즘 : [링크](https://skstp35.tistory.com/231)
>             
>             → IP를 Hash Table에 넣고 거기에서 생성된 값으로 서버를 선정한다.
>             
>             → 따라서 클라이언트에서는 한번 선택된 서버에서 계속해서 서비스를 받게 된다.
>             
> 
> ![Alt text](./Search%20Engine%20Service%20Images/image-7.png)
> 

---

### **LogStash로 MySQL 서버의 로그를 가져와 Elastic Search에 반영**

**[[공식문서 링크](https://guide.ncloud-docs.com/docs/ses-example02)]**

- Logstash Server ID, PW(CentOS)
    
    Kibana Server URI : elastic.sh-bong.site
    
    ID : root
    
    PW : hook1234!
    

- **브로커 노드** : 아마 로그를 가져오는 노드를 말하는듯. **9092 포트**를 쓰는 듯 하다. Cloud Data Streaming Service에서 사용되며 ACG를 따로 잡아주어야 한다.
- **ACG 설정**
    - 우리는 LogStash 전용 서버를 하나 팔 것이다. 돈이 많으니까..
    - 따라서 LogStash 서버에서 Cloud DB Server(우리가 만든 dev db server - MySQL)에도 접근할 수 있어야 하고, 그곳에서 나온 Log들을 Elastic Search에 쏴주어야 하기 때문에 매니저 노드에도 접근할 수 있어야 한다.
    - 또, 보안을 위하여 LogStash 서버는 proxy 서버를 통해서만 들어갈 수 있게 private 서버로 만들었다.
    - 결국 ACG 설정은 다음과 같다.
        - cloud-mysql-xxxxx에 접근소스 logstash server IP + 3306 포트 ACG 설정 → logstash가 3306으로 들어갈 수 있게 함
        - logstash-acg에는 접근소스 proxy server IP + 22 포트 ACG 설정 → Logstash 서버는 프록시 서버를 통해서만 들어갈 수 있고, SSH 통신만 허용한다.
        - searchengine-m-xxxxx에 접근소스 logstash server IP + 9200 포트 ACG 설정 → search engine manager node에도 logstash가 접근하여 mysql server에서 가져온 로그들을 반영시켜 주어야 하기 때문에 manager node에도 ACG를 뚫어주어야 한다.
- 이후 mysql, java 다운로드…는 링크에

```json
위의 Sample Logstash Conf 파일은 5초마다 "statement" 구문이 실행되며
"statement" 구문은 가장 마지막에 조회한 row의 update_time보다 최근 업데이트된 row들을 선택하는 쿼리입니다.
${cdb mysql endpoint} - CDB MySQL의 Private 도메인으로 변경합니다.
${cdb mysql database name} - CDB MySQL 내에 본인이 생성한 Database 이름을 입력합니다.
${cdb user name} - CDB MySQL 내에 본인이 생성한 계정 이름을 입력합니다.
${cdb mysql password} - CDB MySQL 내에 본인이 생성한 계정의 패스워드를 입력합니다.
${ses manager node1 ip} - Search Engine Service 매니저 노드의 IP를 입력합니다.
${ses manager node2 ip} - Search Engine Service 매니저 노드의 IP를 입력합니다(매니저 노드가 이중화되어 있지 않을 경우 입력하지 않습니다)
${userID} - OpenSearch의 경우 클러스터 생성 시 입력한 ID 입니다.
${password} - OpenSearch의 경우 클러스터 생성 시 입력한 password입니다.
```

### 위 작업까지 마치고, MySQL DB Server로부터 LogStash가 로그를 끌어오는지 테스트

- FileBeat 설정?
    
    FileBeat 를 logstash 서버에 깔긴 했는데,,, 이걸 꼭 깔아야 하는지는 잘 모르겠음
    
    안깔아도 상관없음. Logstash가 알아서 log를 가져올 수 있기 때문에.. filebeat는 그냥 로그를 잘 볼 수 있는 툴인듯함
    
- /etc/logstash/conf.d
    
    ```json
    # Sample Logstash configuration for creating a simple
    # Beats -> Logstash -> Elasticsearch pipeline.
    
    input {
        jdbc {
            jdbc_driver_class => "com.mysql.jdbc.Driver"
            jdbc_user => "hook"
            jdbc_password => "hook1234!"
    #       jdbc_driver_class => "com.mysql.jdbc.Driver"
            jdbc_paging_enabled => true
            jdbc_page_size => 50
            statement => "SELECT *, UNIX_TIMESTAMP(update_time) AS unix_ts_in_secs FROM logstash_test WHERE (UNIX_TIMESTAMP(update_time) > :sql_last_value AND update_time < NOW()) ORDER BY update_time ASC"
            record_last_run => true
            clean_run => true
            tracking_column_type => "numeric"
            tracking_column => "unix_ts_in_secs"
            use_column_value => true
            last_run_metadata_path => "/etc/logstash/data/student"
            schedule => "*/5 * * * * *"
      }
    }
    
    output {
            elasticsearch {
                    hosts => ["http://192.168.255.7:9200", "http://192.168.255.8:9200"]
                    index => "cdss-%{+YYYY.MM.dd}"
                    user => "elastic"
                    password => "hook1234!"
             }
    }
    ```
    
    여기에 있는 jdbc의 statement를 잘 만지면 select 문을 활용할 수 있다.
    
    1. [Logstash Configuration](https://www.elastic.co/guide/en/logstash/current/configuration.html): Elastic 사의 Logstash 공식 문서입니다. 설정 파일 작성에 필요한 문법과 옵션에 대한 자세한 정보를 제공합니다.
    2. [Logstash Filters](https://www.elastic.co/guide/en/logstash/current/filter-plugins.html): Logstash에서 사용 가능한 다양한 필터 플러그인에 대한 문서입니다. 각 필터 플러그인의 사용법과 예제를 확인할 수 있습니다.
    3. [Logstash Community Discussions](https://discuss.elastic.co/c/logstash/): Elastic 커뮤니티 포럼에서 Logstash 관련 질문을 하고 답변을 얻을 수 있습니다.
    4. [Logstash GitHub Repository](https://github.com/elastic/logstash): Logstash의 공식 GitHub 저장소입니다. 이곳에서 문서, 예제 및 소스 코드를 확인할 수 있습니다.
    5. [Elastic Community Wiki](https://github.com/elastic/community/wiki): Elastic 커뮤니티 위키는 다양한 Elastic 제품과 관련된 정보를 포함하고 있습니다. Logstash와 관련된 다양한 정보를 찾을 수 있습니다.
    
    ```json
    # 여러가지 결과가 있을 때 다른 인덱스로 보내버리기
    
    input {
       jdbc {
          jdbc_driver_class => "com.mysql.jdbc.Driver"
          # ... 다른 연결 설정 ...
          statement => "SELECT *, 'table1' as source_table FROM table1"
          schedule => "*/5 * * * * *"  # 예: 5분마다 실행
       }
    
       jdbc {
          jdbc_driver_class => "com.mysql.jdbc.Driver"
          # ... 다른 연결 설정 ...
          statement => "SELECT *, 'table2' as source_table FROM table2"
          schedule => "*/5 * * * * *"  # 예: 5분마다 실행
       }
    }
    
    filter {
       if [source_table] == "table1" {
          # table1의 데이터 처리
       }
       else if [source_table] == "table2" {
          # table2의 데이터 처리
       }
    }
    
    output {
       if [source_table] == "table1" {
          elasticsearch {
             hosts => ["http://localhost:9200"]
             index => "index1"
          }
       }
       else if [source_table] == "table2" {
          elasticsearch {
             hosts => ["http://localhost:9200"]
             index => "index2"
          }
       }
    }
    ```
    
- Cloud DB for MySQL 데이터 조회 시 GET ~~~~ : index 이름으로 GET 할 수 있음
    
    ```json
    GET cdss-2023.10.17/_search
    {
      "query": {
        "match_all": {}
      }
    }
    
    -> 
    
    {
      "took" : 5,
      "timed_out" : false,
      "_shards" : {
        "total" : 1,
        "successful" : 1,
        "skipped" : 0,
        "failed" : 0
      },
      "hits" : {
        "total" : {
          "value" : 4,
          "relation" : "eq"
        },
        "max_score" : 1.0,
        "hits" : [
          {
            "_index" : "cdss-2023.10.17",
            "_type" : "_doc",
            "_id" : "XGi3O4sBVpKLJpEuKzKG",
            "_score" : 1.0,
            "_source" : {
              "@timestamp" : "2023-10-17T03:39:01.379Z",
              "@version" : "1",
              "update_time" : "2023-10-16T09:53:14.000Z",
              "contents" : "this is first contents",
              "unix_ts_in_secs" : 1697449994,
              "create_time" : "2023-10-16T09:53:14.000Z",
              "id" : 1
            }
          },
          {
            "_index" : "cdss-2023.10.17",
            "_type" : "_doc",
            "_id" : "2xe3O4sBiKXPZiI3K0sJ",
            "_score" : 1.0,
            "_source" : {
              "@timestamp" : "2023-10-17T03:39:01.391Z",
              "@version" : "1",
              "update_time" : "2023-10-16T09:53:15.000Z",
              "contents" : "this is second contents",
              "unix_ts_in_secs" : 1697449995,
              "create_time" : "2023-10-16T09:53:15.000Z",
              "id" : 2
            }
          },
          {
            "_index" : "cdss-2023.10.17",
            "_type" : "_doc",
            "_id" : "3BciPIsBiKXPZiI3vUsn",
            "_score" : 1.0,
            "_source" : {
              "update_time" : "2023-10-16T09:53:14.000Z",
              "@version" : "1",
              "create_time" : "2023-10-16T09:53:14.000Z",
              "@timestamp" : "2023-10-17T05:36:31.330Z",
              "unix_ts_in_secs" : 1697449994,
              "id" : 1,
              "contents" : "this is first contents"
            }
          },
          {
            "_index" : "cdss-2023.10.17",
            "_type" : "_doc",
            "_id" : "3RciPIsBiKXPZiI3vUsn",
            "_score" : 1.0,
            "_source" : {
              "update_time" : "2023-10-16T09:53:15.000Z",
              "@version" : "1",
              "create_time" : "2023-10-16T09:53:15.000Z",
              "@timestamp" : "2023-10-17T05:36:31.344Z",
              "unix_ts_in_secs" : 1697449995,
              "id" : 2,
              "contents" : "this is second contents"
            }
          }
        ]
      }
    }
    ```
    
- /var/log/logstash/logstash-plain.log → 로그스태시에서 끌어온 로그가 로그스태시 서버에 남는 경로
    
    ```json
    [2023-10-17T14:55:20,052][INFO ][logstash.inputs.jdbc     ][main][9ddaed284537e917942fa6a06518da62cd5fa9312d6f90126c30742c2bbec38b] (0.000577s) SELECT version()
    [2023-10-17T14:55:20,054][INFO ][logstash.inputs.jdbc     ][main][9ddaed284537e917942fa6a06518da62cd5fa9312d6f90126c30742c2bbec38b] (0.000476s) SELECT version()
    [2023-10-17T14:55:20,056][INFO ][logstash.inputs.jdbc     ][main][9ddaed284537e917942fa6a06518da62cd5fa9312d6f90126c30742c2bbec38b] (0.000774s) SELECT count(*) AS `count` FROM (SELECT *, UNIX_TIMESTAMP(update_time) AS unix_ts_in_secs FROM logstash_test WHERE (UNIX_TIMESTAMP(update_time) > 1697449995 AND update_time < NOW()) ORDER BY update_time ASC) AS `t1` LIMIT 1
    [2023-10-17T14:55:37,136][WARN ][logstash.config.source.multilocal] Ignoring the 'pipelines.yml' file because modules or command line options are specified
    [2023-10-17T14:55:37,274][INFO ][logstash.runner          ] Starting Logstash {"logstash.version"=>"7.7.0"}
    [2023-10-17T14:55:39,519][INFO ][org.reflections.Reflections] Reflections took 43 ms to scan 1 urls, producing 21 keys and 41 values
    [2023-10-17T14:55:40,486][INFO ][logstash.outputs.elasticsearch][main] Elasticsearch pool URLs updated {:changes=>{:removed=>[], :added=>[http://elastic:xxxxxx@192.168.255.7:9200/, http://elastic:xxxxxx@192.168.255.8:9200/]}}
    [2023-10-17T14:55:40,739][WARN ][logstash.outputs.elasticsearch][main] Restored connection to ES instance {:url=>"http://elastic:xxxxxx@192.168.255.7:9200/"}
    [2023-10-17T14:55:40,791][INFO ][logstash.outputs.elasticsearch][main] ES Output version determined {:es_version=>7}
    [2023-10-17T14:55:40,796][WARN ][logstash.outputs.elasticsearch][main] Detected a 6.x and above cluster: the `type` event field won't be used to determine the document _type {:es_version=>7}
    [2023-10-17T14:55:40,810][WARN ][logstash.outputs.elasticsearch][main] Restored connection to ES instance {:url=>"http://elastic:xxxxxx@192.168.255.8:9200/"}
    [2023-10-17T14:55:40,849][INFO ][logstash.outputs.elasticsearch][main] New Elasticsearch output {:class=>"LogStash::Outputs::ElasticSearch", :hosts=>["http://192.168.255.7:9200", "http://192.168.255.8:9200"]}
    [2023-10-17T14:55:40,912][INFO ][logstash.outputs.elasticsearch][main] Using default mapping template
    [2023-10-17T14:55:40,994][WARN ][org.logstash.instrument.metrics.gauge.LazyDelegatingGauge][main] A gauge metric of an unknown type (org.jruby.specialized.RubyArrayOneObject) has been created for key: cluster_uuids. This may result in invalid serialization.  It is recommended to log an issue to the responsible developer/development team.
    [2023-10-17T14:55:41,000][INFO ][logstash.outputs.elasticsearch][main] Index Lifecycle Management is set to 'auto', but will be disabled - Index Lifecycle management is not installed on your Elasticsearch cluster
    [2023-10-17T14:55:41,004][INFO ][logstash.outputs.elasticsearch][main] Attempting to install template {:manage_template=>{"index_patterns"=>"logstash-*", "version"=>60001, "settings"=>{"index.refresh_interval"=>"5s", "number_of_shards"=>1}, "mappings"=>{"dynamic_templates"=>[{"message_field"=>{"path_match"=>"message", "match_mapping_type"=>"string", "mapping"=>{"type"=>"text", "norms"=>false}}}, {"string_fields"=>{"match"=>"*", "match_mapping_type"=>"string", "mapping"=>{"type"=>"text", "norms"=>false, "fields"=>{"keyword"=>{"type"=>"keyword", "ignore_above"=>256}}}}}], "properties"=>{"@timestamp"=>{"type"=>"date"}, "@version"=>{"type"=>"keyword"}, "geoip"=>{"dynamic"=>true, "properties"=>{"ip"=>{"type"=>"ip"}, "location"=>{"type"=>"geo_point"}, "latitude"=>{"type"=>"half_float"}, "longitude"=>{"type"=>"half_float"}}}}}}}
    [2023-10-17T14:55:41,014][INFO ][logstash.javapipeline    ][main] Starting pipeline {:pipeline_id=>"main", "pipeline.workers"=>4, "pipeline.batch.size"=>125, "pipeline.batch.delay"=>50, "pipeline.max_inflight"=>500, "pipeline.sources"=>["/etc/logstash/conf.d/logstash.conf"], :thread=>"#<Thread:0x328dcad4 run>"}
    [2023-10-17T14:55:42,136][INFO ][logstash.javapipeline    ][main] Pipeline started {"pipeline.id"=>"main"}
    [2023-10-17T14:55:42,250][INFO ][logstash.agent           ] Pipelines running {:count=>1, :running_pipelines=>[:main], :non_running_pipelines=>[]}
    [2023-10-17T14:55:42,957][INFO ][logstash.agent           ] Successfully started Logstash API endpoint {:port=>9600}
    ```
    
- Kibana에서 Dev Tools - Console에서 검색 테스트
    
    ![Alt text](./Search%20Engine%20Service%20Images/image-8.png)
    
    쿼리문은 /etc/logstash/conf.d의 statement에서 수정할 수 있음.
    

---

### SpringBoot와 ElasticSearch 연동해서 사용하기

### 🏫 전제

![Alt text](./Search%20Engine%20Service%20Images/image-9.png)

우선 ElasticSearch는 NoSQL 방식이므로, JPA와 호환되지 않는다.

따라서 우리는 ElasticsearchRepository를 상속받는 ElasticSearch 전용 Repository를 따로 만들어 주어야 하며, Repository에 해당하는 Entity는 다음과 같이 만들 수 있다.

### 💡Entity 만들기

```json
@Document(indexName = "index1")
public class YourEntity {
    @Id
    private String id;
    // 다른 필드들...
}
```

`@Document` 어노테이션의 속성으로 엔티티로 관리할 특정 index를 지정할 수 있다.

저 위에 conf 파일에서 output으로 만든 index의 이름을 잡아 그 index 자체를 Entity화 할 수 있다.

### 💡이후 자바 코드

`Entity` 와 `Repository` 를 만들었다면 이후에는 Controller와 Service 등을 만들어 API를 작성하면 되겠다.

---

## Kibana

[Kibana 기능 목록 | Elastic](https://www.elastic.co/kr/kibana/features)

[Kibana/OpenSearch 활용](https://guide.ncloud-docs.com/docs/ses-kibana-vpc)

Elastic Search 관리 & 상황 모니터링 등등..을 할 수 있는 인터페이스 제공

**Port : 5601**

ID : hook

PWD : hook1234!

> **Kibana domain 만들기**
> 
> 
> ![Alt text](./Search%20Engine%20Service%20Images/image-10.png)
> 
> 위처럼 elastic.sh-bong.site로 접속했을 때 Kibana로 들어오도록 설정할 수 있다.
> 
> ![Alt text](./Search%20Engine%20Service%20Images/image-11.png)
> 
> elastic.sh-bong.site로 접속했을 때, 우리가 만들어준 NLB를 타게 하면 된다.