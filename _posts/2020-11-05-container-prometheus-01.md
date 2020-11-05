---
layout: post
title: 'Prometheus의 기본 구성 요소'
subtitle: 'Prmetheus의 기본 구성 요소에 대해 알아보자'
categories: container
tags: prometheus
comments: false
---

### 프로메테우스 구성요소 ###
![프로메테우스 구조](/assets/img/prometheus/prometheus-01.PNG){: width="600" height="400"}

1) Prometheus server
- 시계열 데이터를 수집해 와서 저장한다. Retrieval, TSDB, HTTP Server로 구성되어 있다. 프로메테우스는 Pull 방식을 사용하여 일정 주기마다 서버의 클라이언트에 접속하여 데이터를 수집한다.(이를 스크랩이라 한다.)
예를 들어,쿠버네티스의자원을 모니터링 하는 경우 프로메테우스 서버는 각 노드의 kubelet으로부터 리소스 정보를 스크랩하게 된다. 

2) Pushgateway 
- 크론(cron)이나 배치 작업이 메트릭 정보를 프로메테우스로 게시할 수 있도록 도와준다. 이런 작업들은 프로메테우스가 스크랩 할 수 있을만큼 오래 존재하지 않을 수 있으므로, 프로메테우스가 작업 서버를 직접 스크랩하는 대신, 작업을 담당하는 서버가 게시하고자 하는 메트릭을 pushgateway로 보내면, 프로메테우스 서버가 이를 스크랩하게 된다. 

3) Exporter
- Exporter는 프로메테우스와 호환되지 않는 애플리케이션의 메트릭을 노출할 수 있도록 도와주는 도구이다. 익스포터는 서비스나 애플리케이션에서 데이터를 수집하고 프로메테우스와 호환되는 형식으로 노출한다. 예를 들어 Node Exporter는 서버의 커널로부터 통계를 수집하여 디스크 I/O, CPU, 메모리, 네트워크, 파일 시스템 사용량 등과 같은 데이터를 HTTP Method로 노출한다. 현재는 많은 서비스에 대한 익스포터가 third party에 의해 개발되어 있다. (https://prometheus.io/docs/instrumenting/exporters/)

4) Alertmanager
- 프로메테우스 서버에서 생성된 알림 트리거를 수신하여 메일과 같은 다른 서비스로 통지(Notification)를 담당한다. 

5) Service discovery
- 프로메테우스는 기본적으로 스크랩 대상에 대한 정보를 설정 파일에 등록하여 가지고 있다. 하지만 이 방법은 동적 환경을 다룰 때 적절한 옵션이 아니다. 이를 대체하기 위해 서비스 디스커버리를 사용한다. 서비스 디스커버리가 동적으로 생성, 삭제되는 타겟 정보를 관리하고 프로메테우스 서버는 이 목록으로부터 메트릭을 스크랩하게 된다.   

5) API clients
### 참고 자료
- https://github.com/prometheus/pushgateway
- https://prometheus.io/docs/introduction/overview/
- https://prometheus.io/docs/instrumenting/exporters/
- 프로메테우스 인프라스트럭처 모니터링[조엘 바스토스 외, 에이콘]