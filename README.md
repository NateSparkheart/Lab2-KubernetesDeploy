Технологии разработки программного обеспечения

Лабораторная работа №2

Cоздание кластера Kubernetes и деплой приложения

Выполнил: студент группы МБД 2131 Савицкий В.Д.

Цель работы работы: знакомство с кластерной архитектурой на примере Kubernetes, а также деплоем приложения в кластер.

Манифесты

deployment.yaml:

    apiVersion: apps/v1
    kind: Deployment
    metadata:
      name: my-deployment
    spec:
      replicas: 10
      selector:
        matchLabels:
          app: my-app
      strategy:
        rollingUpdate:
          maxSurge: 1
          maxUnavailable: 1
        type: RollingUpdate
      template:
        metadata:
          labels:
            app: my-app
        spec:
          containers:
            - image: myapi:latest
              # https://medium.com/bb-tutorials-and-thoughts/how-to-use-own-local-doker-images-with-minikube-2c1ed0b0968
              # указыаает на то, что образы нужно брать только из локального registry. В продакшене никогда не использовать
              imagePullPolicy: Never 
              name: myapi
              ports:
                - containerPort: 8080
          hostAliases:
          - ip: "192.168.56.1" # The IP of localhost from MiniKube
            hostnames:
            - postgres.local
                   
service.yaml:

    apiVersion: v1
    kind: Service
    metadata:
      name: my-service
    spec:
      type: NodePort
      ports:
        - nodePort: 31317
          port: 8080
          protocol: TCP
          targetPort: 8080
      selector:
        app: my-app

Состояние подов в консоли:

![Image alt](https://github.com/NateSparkheart/Lab2-KubernetesDeploy/blob/main/Lab2-Consoleshot.png?raw=true)

Состояние подов в графическом интерфейсе:

![Image alt](https://github.com/NateSparkheart/Lab2-KubernetesDeploy/blob/main/Lab2-Dashboardshot.png?raw=true)
