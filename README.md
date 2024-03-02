### Opensearch Docker Install

#### 이슈-1 : ADMIN 비번 이슈
 - docker-compose.yml 설정시 ADMIN_PASSWORD를 설정안하면 opensearch 엔진이 살지 않는다.
 - 따라서 반드시 환경번수 절에 비번을 설정해야 한다.

```
    environment:
      - "OPENSEARCH_JAVA_OPTS=-Xms512m -Xmx512m"
      - OPENSEARCH_INITIAL_ADMIN_PASSWORD=Ghdrlfehd1   <-- 이때 비번을 <대문자><소문자><숫자> 등을 섞어서 강력하게 지정해야 제대로 기동이 됨
```

#### 이슈-2 : SSL 이슈
 - opensearch와 dashboard 등 서비스들 간에 통신은 반드시 SSL로 해야한다.
 - 따라서 다음 dashboard 서비스 절에서 HTTPS 로 반드 설정해야 서로 통신이 가능하게 된다.

```
  opensearch-dashboards:
    image: opensearchproject/opensearch-dashboards:latest # Make sure the version of opensearch-dashboards matches the version of opensearch installed on other nodes
    container_name: opensearch-dashboards
    ports:
      - 5601:5601 # Map host port 5601 to container port 5601
    expose:
      - "5601" # Expose port 5601 for web access to OpenSearch Dashboards
    environment:
      OPENSEARCH_HOSTS: '["https://opensearch-node1:9200","https://opensearch-node2:9200"]' <--- HERE 
    networks:
      - opensearch-net
```
