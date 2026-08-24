---
title:  "#02 Google Cloud 로그 기초"
categories:
    [Log Analysis]
tags:
  - [Log, Google Cloud]

toc: true
toc_sticky: true
 
date: 2026-08-24
last_modified_at: 2026-08-24
---

Google Cloud는 이번에 처음 접해본다. 사실 Cloud를 학습해 본 경험이 많지 않기 때문에... 구글 스터디잼에 참여하게 된 겸 한번 차근차근 공부해 보려고 한다.

## 1. 로그를 확인하는 법
가상 머신을 만들고 방화벽 규칙을 설정하는 방법까지 알고 있다는 전제 하에, 로그는 이렇게 확인할 수 있다.

```sh
gcloud logging logs list --filter="compute" //컴퓨팅 리소스와 관련된 로그를 확인
```

```sh
gcloud logging read "resource.type=gce_instance" --limit 5 // gce_instance 리소스 유형과 관련된 로그 읽기
```

```sh
gcloud logging read "resource.type=gce_instance AND labels.instance_name=[가상머신 이름]" --limit 5 //특정 가상 머신의 로그 읽기
```

맨 처음 뜬 로그를 가져와보자.

## 2. 로그
```
---
insertId: -p7n95bd54h0
labels:
  compute.googleapis.com/root_trigger_id: [REDACTED]
logName: projects/qwiklabs-gcp-04-XXXXXXXXXXXX/logs/cloudaudit.googleapis.com%2Factivity
operation:
  id: [REDACTED]
  last: true
  producer: compute.googleapis.com
protoPayload:
  '@type': type.googleapis.com/google.cloud.audit.AuditLog
  apiVersionIdentifier: v1
  authenticationInfo:
    oauthInfo:
      oauthClientId: [REDACTED]
    principalEmail: student-01-************@qwiklabs.net
    principalSubject: user:student-01-************@qwiklabs.net
  methodName: v1.compute.instances.setTags
  request:
    '@type': type.googleapis.com/compute.instances.setTags
  requestMetadata:
    callerIp: 35.229.xxx.xxx
    callerSuppliedUserAgent: google-cloud-sdk gcloud/581.0.0 command/gcloud.compute.instances.add-tags
      invocation-id/[REDACTED] environment/devshell environment-version/None
      client-os/LINUX client-os-ver/6.6.143 client-pltf-arch/x86_64 interactive/True
      from-script/False python/3.14.6 term/tmux-256color  (Linux 6.6.143+),gzip(gfe)
    destinationAttributes: {}
    requestAttributes: {}
  resourceName: projects/qwiklabs-gcp-04-XXXXXXXXXXXX/zones/us-west1-c/instances/gcelab2
  serviceName: compute.googleapis.com
  status: {}
receiveTimestamp: '2026-08-24T07:27:03.142826199Z'
resource:
  labels:
    instance_id: '[REDACTED]'
    project_id: qwiklabs-gcp-04-XXXXXXXXXXXX
    zone: us-west1-c
  type: gce_instance
severity: NOTICE
timestamp: '2026-08-24T07:27:02.136622Z'
---
```
전체적으로 읽는 게 어렵지는 않지만 그래도 gcloud 고유의 필드가 몇 개 있는 것 같다.

우선 대략적으로 읽어보자면, 내가 gcloud로 gcelab2라는 가상머신의 Network Tag를 변경한 기록인 것 같다.

> Network Tag를 변경했다는 건 가상머신에 적용되는 방화벽 규칙에 변동이 생겼을 수 있다는 의미이므로 보안적으로 중요하다고 볼 수 있겠다.

```sh
methodName: v1.compute.instances.setTags
```
이 필드를 보면 쉽게 알 수 있다.

```sh
callerSuppliedUserAgent:
  google-cloud-sdk gcloud/581.0.0
  command/gcloud.compute.instances.add-tags
```
여기에는 내가 사용한 명령인 `gcloud compute instances add-tags`도 볼 수 있다.

로그를 한번 전체적으로 살펴봤으니, 이제 내일은 어떻게 로그를 더 상세하게 읽어낼 수 있을지를 살펴보자.