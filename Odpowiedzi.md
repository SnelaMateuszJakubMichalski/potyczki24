Zad. 1 

Za pomocą GUI Ranchera stworzyliśmy projekt "web-server" 

kubectl create namespace nginx 

kubectl create deployment nginx --image=nginx --namespace=nginx --replicas=3 

kubectl expose deployment nginx --namespace=nginx --port=80 --target-port=80 --name=nginx-service --type=NodePort 

kubectl expose service nginx-service --namespace=nginx --type=NodePort --name=nginx-nodeport 

 

kubectl get nodes -o wide -> pobranie zewnętrznego adresu klastra 

 

Za pomocą GUI Ranchera przenieśliśmy namespace nginx do projektu 

 

Udostępniliśmy klaster pod adresem: http://ngnix-team33.duckdns.org:30164/ 

Zad.2 

 

 

curl -sSfL https://raw.githubusercontent.com/longhorn/longhorn/v1.6.1/scripts/environment_check.sh | bash 

 

[ERROR] Not found: jq 
[INFO]  Please install missing dependencies: kubectl jq mktemp sort printf. 

 

 

 

Zad.3 

helm repo add tetris-repo https://rancher.github.io/rodeo 

helm repo update 

kubectl create namespace tetris 

helm install tetris tetris-repo/tetris --namespace tetris 

 

 

 

Zad.4 

Instalacja przez GUI Ranchera 
zalogowanie sie do gui neuvectora 

 

 

Zad.6 

 

Aby zachować PersistentVolumeClaim (PVC) i automatycznie podłączyć się do niego ponownie po przeskalowaniu do 0 i wznowieniu usługi, możemy skorzystać z mechanizmu PersistentVolume (PV) reclaims. PV reclaims umożliwiają ponowne wykorzystanie istniejącego PV przez PVC nawet po tym, jak zasoby PV zostały zwolnione. 

W celu skorzystania z tej funkcji, należy ustawić właściwość persistentVolumeReclaimPolicy dla PV na wartość Retain. W ten sposób PV nie zostanie automatycznie usunięty po tym, jak PVC zostanie usunięte. Następnie, gdy PVC zostanie ponownie utworzone, może ono użyć istniejącego PV. 

Oto przykładowy kod YAML, który demonstruje użycie tej metody: 

 

apiVersion: apps/v1 

kind: StatefulSet 

metadata: 

  name: ubuntu-statefulset 

spec: 

  replicas: 1 

  serviceName: "ubuntu" 

  selector: 

    matchLabels: 

      app: ubuntu 

  template: 

    metadata: 

      labels: 

        app: ubuntu 

    spec: 

      terminationGracePeriodSeconds: 10 

      containers: 

      - name: ubuntu 

        image: busybox:latest 

        command: 

        - sleep 

        - "86400" 

        volumeMounts: 

        - mountPath: /shared_files 

          name: test-vol 

  volumeClaimTemplates: 

  - metadata: 

      name: test-vol 

    spec: 

      accessModes: [ "ReadWriteOnce" ] 

      resources: 

        requests: 

          storage: 1Gi 

 

 

 

Zad. 7 

kubectl get deployment nazwa_deploymentu -o yaml 

 

Lub w rancherze wejść na rancherze wejść na deployments, wybrać tan, który chcemy sprawdzić i po prawej u góry wybrać z opcji (3 kropki) "download yaml" 

 

Zad.8 

Uruchomienie kontenera MySQL na najprostszych domyślnych ustawieniach może być wygodne na początku, ale nie jest zalecane w środowisku produkcyjnym. Domyślne ustawienia mogą być niewystarczające w kontekście wydajności, bezpieczeństwa i dostępności. Na przykład, domyślne ustawienia mogą nie uwzględniać optymalizacji wydajności, takich jak rozmiar bufora, co może prowadzić do spowolnienia działania aplikacji w miarę wzrostu obciążenia. Co więcej, domyślne ustawienia bezpieczeństwa mogą być podatne na ataki, zwłaszcza w przypadku środowisk dostępnych publicznie. Dlatego zaleca się dostosowanie konfiguracji MySQL do indywidualnych potrzeb aplikacji, w tym optymalizacji wydajności, zabezpieczeń i dostępności. Można to osiągnąć poprzez dostosowanie parametrów konfiguracyjnych MySQL, takich jak wielkość bufora, limit połączeń i mechanizmy uwierzytelniania. Zaleca się również korzystanie z zarządzanych usług bazodanowych, które mogą automatycznie dostosować się do zmieniających się potrzeb i zapewnić lepsze wsparcie w zakresie bezpieczeństwa i dostępności. 

 

apiVersion: apps/v1 

kind: Deployment 

metadata: 

  annotations: 

    deployment.kubernetes.io/revision: '1' 

    objectset.rio.cattle.io/applied: >- 

      H4sIAAAAAAAA/4SR32obPRDF32Wu186u1+tIuvsglwn+GpeWUkKY1Y5rtfoXadZgjN69aCnF0JReSaM5nPnN0RUwmk+UsgkeFGCM+e7cQQM/jJ9AwQNFGy6OPEMDjhgnZAR1BfQ+MLIJPtcyjN9JcyZeJxPWGpktrU24M9Xk2Hc47cSw6vVwXG3luFkJIdqVnvpBSiHboeugNGBxJLvYYYygwF3ym4Xmr+YnzCdQoCfZd8ceRS9Jis290DvcCjFuRbcZ5LiZ+lbf60nWER4d3TjXMkfU9W2iI86WqypH0hUjUbRGYwbVNZDJkuaQasMh69Pju7ilNMDkokWmRXoT2vsLlpuJOnhG4yllUF+vQP68nL+wn74cPjy+Pu/3H1///+9w+Lx/foAGzmjn2s2kE3HEnKG8NGAcfvu9q6o4meGPBBLlMCdNC5U1zvBy03EGBV3buuXfXUgXULDbPpmaT6K3mfK/laW8lFLKzwAAAP//3eafh2UCAAA 

    objectset.rio.cattle.io/id: f31ad685-3c5f-49b2-8880-cd3599890511 

  creationTimestamp: '2024-04-08T08:01:16Z' 

  generation: 1 

  labels: 

    app: mysql 

    objectset.rio.cattle.io/hash: cd931f3a839e98278c6a488b481259b2d30c7cd9 

  managedFields: 

    - apiVersion: apps/v1 

      fieldsType: FieldsV1 

      fieldsV1: 

        f:metadata: 

          f:annotations: 

            .: {} 

            f:objectset.rio.cattle.io/applied: {} 

            f:objectset.rio.cattle.io/id: {} 

          f:labels: 

            .: {} 

            f:app: {} 

            f:objectset.rio.cattle.io/hash: {} 

        f:spec: 

          f:progressDeadlineSeconds: {} 

          f:replicas: {} 

          f:revisionHistoryLimit: {} 

          f:selector: {} 

          f:strategy: 

            f:rollingUpdate: 

              .: {} 

              f:maxSurge: {} 

              f:maxUnavailable: {} 

            f:type: {} 

          f:template: 

            f:metadata: 

              f:labels: 

                .: {} 

                f:app: {} 

            f:spec: 

              f:containers: 

                k:{"name":"mysql"}: 

                  .: {} 

                  f:env: 

                    .: {} 

                    k:{"name":"MYSQL_ROOT_PASSWORD"}: 

                      .: {} 

                      f:name: {} 

                      f:value: {} 

                  f:image: {} 

                  f:imagePullPolicy: {} 

                  f:name: {} 

                  f:resources: 

                    .: {} 

                    f:limits: 

                      .: {} 

                      f:cpu: {} 

                      f:memory: {} 

                    f:requests: 

                      .: {} 

                      f:cpu: {} 

                      f:memory: {} 

                  f:terminationMessagePath: {} 

                  f:terminationMessagePolicy: {} 

              f:dnsPolicy: {} 

              f:restartPolicy: {} 

              f:schedulerName: {} 

              f:securityContext: {} 

              f:terminationGracePeriodSeconds: {} 

      manager: agent 

      operation: Update 

      time: '2024-04-08T08:01:16Z' 

    - apiVersion: apps/v1 

      fieldsType: FieldsV1 

      fieldsV1: 

        f:metadata: 

          f:annotations: 

            f:deployment.kubernetes.io/revision: {} 

        f:status: 

          f:conditions: 

            .: {} 

            k:{"type":"Available"}: 

              .: {} 

              f:lastTransitionTime: {} 

              f:lastUpdateTime: {} 

              f:message: {} 

              f:reason: {} 

              f:status: {} 

              f:type: {} 

            k:{"type":"Progressing"}: 

              .: {} 

              f:lastTransitionTime: {} 

              f:lastUpdateTime: {} 

              f:message: {} 

              f:reason: {} 

              f:status: {} 

              f:type: {} 

          f:observedGeneration: {} 

          f:replicas: {} 

          f:unavailableReplicas: {} 

          f:updatedReplicas: {} 

      manager: kube-controller-manager 

      operation: Update 

      subresource: status 

      time: '2024-04-08T09:13:24Z' 

  name: mysql 

  namespace: default 

  resourceVersion: '51499' 

  uid: 2cf31ea7-d1fc-484c-adbe-3e4304b7eec4 

spec: 

  progressDeadlineSeconds: 600 

  replicas: 1 

  revisionHistoryLimit: 10 

  selector: 

    matchLabels: 

      app: mysql 

  strategy: 

    rollingUpdate: 

      maxSurge: 25% 

      maxUnavailable: 25% 

    type: RollingUpdate 

  template: 

    metadata: 

      creationTimestamp: null 

      labels: 

        app: mysql 

    spec: 

      containers: 

        - env: 

            - name: MYSQL_ROOT_PASSWORD 

              value: secretpass 

          image: mysql:latest 

          imagePullPolicy: Always 

          name: mysql 

          resources: 

            limits: 

              cpu: 100m 

              memory: 64Mi 

            requests: 

              cpu: 100m 

              memory: 64Mi 

          terminationMessagePath: /dev/termination-log 

          terminationMessagePolicy: File 

      dnsPolicy: ClusterFirst 

      restartPolicy: Always 

      schedulerName: default-scheduler 

      securityContext: {} 

      terminationGracePeriodSeconds: 30 

status: 

  conditions: 

    - lastTransitionTime: '2024-04-08T08:01:16Z' 

      lastUpdateTime: '2024-04-08T08:01:36Z' 

      message: ReplicaSet "mysql-85b68446c6" has successfully progressed. 

      reason: NewReplicaSetAvailable 

      status: 'True' 

      type: Progressing 

    - lastTransitionTime: '2024-04-08T09:13:24Z' 

      lastUpdateTime: '2024-04-08T09:13:24Z' 

      message: Deployment does not have minimum availability. 

      reason: MinimumReplicasUnavailable 

      status: 'False' 

      type: Available 

  observedGeneration: 1 

  replicas: 1 

  unavailableReplicas: 1 

  updatedReplicas: 1 

 

 

Zad.9 

Gateway w Kubernetes to zasób, który umożliwia zarządzanie ruchem sieciowym do aplikacji działających w klastrze. Działa jako punkt wejścia dla ruchu z zewnątrz klastra do aplikacji wewnątrz klastra, umożliwiając równoważenie obciążenia, routowanie ruchu i kontrolę dostępu 

 

Zadanie 

Deployment to sposób na zarządzanie aplikacją w Kubernetes, natomiast ReplicaSet kontroluje ilość replik tych samych podów, zapewniając ich nieprzerwane działanie. 

 

 

Zad.10 

Rancher GUI -> "Cluster Management" -> "Edit Config". Następnie zmieniamy wersję kubernetes i zapisujemys 

 

Zad. 11 

 

# konto dla nowego dyrektora IT 

kubectl create serviceaccount muhammed.yassuff 

  

# konto dla nowego praktykanta 

kubectl create serviceaccount muhhamad.yussuff 

 

 

#uprawnienia do dyrektora IT 

kubectl create clusterrolebinding muhammed-yassuff-view \ 

  --clusterrole=view \ 

  --serviceaccount=default:muhammed.yassuff 

 

# uprawnienia do praktykanta 

kubectl create clusterrolebinding muhhamad-yussuff-edit \ 

  --clusterrole=edit \ 

  --serviceaccount=default:muhhamad.yussuff 

 

 

Zad. 12 

 
zad13 
![image](https://github.com/SnelaMateuszJakubMichalski/potyczki24/assets/166373287/ce7575dd-0f46-4744-a971-439a728e8df7)

 

Zad. 14 
kubectl create namespace adrian 

 

Vi adrian-nginx.yaml: 

 

apiVersion: apps/v1 
kind: Deployment 
metadata: 
   name: nginx 
   labels: 
     app: nginx 
spec: 
   replicas: 1 
   selector: 
     matchLabels: 
       app: nginx 
   template: 
     metadata: 
       labels: 
         app: nginx 
     spec: 
       containers: 
       - name: nginx 
         image: nginx:latest 
         ports: 
         - containerPort: 80 
         readinessProbe: 
           tcpSocket: 
             port: 80 
           initialDelaySeconds: 5 
           periodSeconds: 10 
         livenessProbe: 
           httpGet: 
             path: / 
             port: 80 
           initialDelaySeconds: 10 
           periodSeconds: 15 

 

 

kubectl apply -f adrian-nginx.yaml -n adrian 

 

Zad.16 

kubectl create namespace serwis 

  

kubectl apply -f serwis.yaml -n serwis 

  

  

apiVersion: apps/v1 
kind: Deployment 
metadata: 
   name: nginx 
   labels: 
     app: nginx 
spec: 
   replicas: 1 
   selector: 
     matchLabels: 
       app: nginx 
   template: 
     metadata: 
       labels: 
         app: nginx 
     spec: 
       containers: 
       - name: nginx 
         image: nginx:latest 
         ports: 
         - containerPort: 80 
--- 
apiVersion: v1 
kind: Service 
metadata: 
   name: nginx-svc 
   namespace: serwis 
spec: 
   selector: 
     app: nginx 
   ports: 
     - protocol: TCP 
       port: 80 
       targetPort: 80 

 

 

Zad. 17 

kubectl create namespace baza 

  

kubectl apply -f baza.yaml -n baza 

  

apiVersion: apps/v1 
kind: Deployment 
metadata: 
   name: mysql 
   labels: 
     app: mysql 
spec: 
   replicas: 1 
   selector: 
     matchLabels: 
       app: mysql 
   template: 
     metadata: 
       labels: 
         app: mysql 
     spec: 
       containers: 
       - name: mysql 
         image: mysql:latest 
         env: 
           - name: MYSQL_ROOT_PASSWORD 
             value: secretpass 
         resources: 
           requests: 
             memory: "512Mi" 
             cpu: "100m" 
           limits: 
             memory: "512Mi" 
             cpu: "100m" 

 
 zad 18
Problemem jest to ze została podana błędna architektura plików
Zmiana container-image na rancher/hello-world
 

 

 

 

 

 

 

 

 

 

 

 
