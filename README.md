### pod 확인
$ kubectl get pods
NAME                          READY   STATUS    RESTARTS   AGE
nginx-test-686f6ff54d-gkk9h   1/1     Running   0          29m

### pod 상세정보 - 도커 컨테이너 정보
$ kubectl describe pod nginx-test-686f6ff54d-gkk9h

### 해당 도커 컨테이너로 접속
# $ kubectl exec -it <pod-name> -c <container-name> -- /bin/bash
$ kubectl exec -it nginx-test-686f6ff54d-gkk9h -c nginx-test -- /bin/bash

### 로그확인
$ kubectl logs -f nginx-test-686f6ff54d-gkk9h

### 생성 및 변경 적용
$ kubectl apply -f httpd-deployment.yaml
deployment.apps/httpd-deployment created
service/httpd-service created

### github raw url
$ kubectl apply -f https://raw.githubusercontent.com/dMario24/k2s/refs/tags/v1.1.1/httpd-deployment.yaml
deployment.apps/httpd-deployment unchanged
service/httpd-service unchanged

### 배포로 생성된 Pod를 나열
$ kubectl get pods -l app=httpd
NAME                                READY   STATUS    RESTARTS   AGE
httpd-deployment-599bf897b4-m9vn5   1/1     Running   0          15m

### scale in/out
$ kubectl scale deployment httpd-deployment --replicas=3

### 상세정보
$ kubectl describe pod httpd-deployment-599bf897b4-m9vn5
Name:             httpd-deployment-599bf897b4-m9vn5
Namespace:        default
Priority:         0
Service Account:  default
Node:             minikube/192.168.49.2
Start Time:       Fri, 01 Nov 2024 11:50:57 +0900
Labels:           app=httpd
                  pod-template-hash=599bf897b4
Annotations:      <none>
Status:           Running
IP:               10.244.0.7
IPs:
  IP:           10.244.0.7
Controlled By:  ReplicaSet/httpd-deployment-599bf897b4
Containers:
  httpd:
    Container ID:   docker://b7fb2de81541d4d3519e31d43eacbb5684ad2b061d6ac5806074cad370676bc4
    Image:          httpd
    Image ID:       docker-pullable://httpd@sha256:bbea29057f25d9543e6a96a8e3cc7c7c937206d20eab2323f478fdb2469d536d
    Port:           80/TCP
    Host Port:      0/TCP
    State:          Running
      Started:      Fri, 01 Nov 2024 11:51:05 +0900
    Ready:          True
    Restart Count:  0
    Environment:    <none>
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-dth9d (ro)
Conditions:
  Type                        Status
  PodReadyToStartContainers   True
  Initialized                 True
  Ready                       True
  ContainersReady             True
  PodScheduled                True
Volumes:
  kube-api-access-dth9d:
    Type:                    Projected (a volume that contains injected data from multiple sources)
    TokenExpirationSeconds:  3607
    ConfigMapName:           kube-root-ca.crt
    ConfigMapOptional:       <nil>
    DownwardAPI:             true
QoS Class:                   BestEffort
Node-Selectors:              <none>
Tolerations:                 node.kubernetes.io/not-ready:NoExecute op=Exists for 300s
                             node.kubernetes.io/unreachable:NoExecute op=Exists for 300s
Events:
  Type    Reason     Age   From               Message
  ----    ------     ----  ----               -------
  Normal  Scheduled  15m   default-scheduler  Successfully assigned default/httpd-deployment-599bf897b4-m9vn5 to minikube
  Normal  Pulling    15m   kubelet            Pulling image "httpd"
  Normal  Pulled     15m   kubelet            Successfully pulled image "httpd" in 6.973s (6.973s including waiting). Image size: 148178227 bytes.
  Normal  Created    15m   kubelet            Created container httpd
  Normal  Started    15m   kubelet            Started container httpd

### 삭제
$ kubectl delete deployment nginx-deployment

