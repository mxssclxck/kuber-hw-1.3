# Никоноров Денис - FOPS-8
# Домашнее задание к занятию «Запуск приложений в K8S»

### Задание 1. Создать Deployment и обеспечить доступ к репликам приложения из другого Pod

1. Создать Deployment приложения, состоящего из двух контейнеров — nginx и multitool. Решить возникшую ошибку.
2. После запуска увеличить количество реплик работающего приложения до 2.
3. Продемонстрировать количество подов до и после масштабирования.
4. Создать Service, который обеспечит доступ до реплик приложений из п.1.
5. Создать отдельный Pod с приложением multitool и убедиться с помощью `curl`, что из пода есть доступ до приложений из п.1.

------

### Задание 2. Создать Deployment и обеспечить старт основного контейнера при выполнении условий

1. Создать Deployment приложения nginx и обеспечить старт контейнера только после того, как будет запущен сервис этого приложения.
2. Убедиться, что nginx не стартует. В качестве Init-контейнера взять busybox.
3. Создать и запустить Service. Убедиться, что Init запустился.
4. Продемонстрировать состояние пода до и после запуска сервиса.

---

### Решение задания 1. Создать Deployment и обеспечить доступ к репликам приложения из другого Pod

1. Создаю отдельный Namespace, чтобы созданные в этом задание pods, deployments, services работали отдельно от остальных

![alt text](img/1.png)

Далее манифест Deployment приложения [deployment-nginx-multitool](/deployment-nginx-multitool.ymldeployment-nginx)

Произвожу его запуск

![alt text](img/2.png)

Проверка

![alt text](img/3.png)

2. Запущена одна реплика приложения nginx-multitool. Увеличение кол-ва реплик можно произвести через изменение файла [deployment-nginx-multitool](/deployment-nginx-multitool.ymldeployment-nginx) `replicas:1` меняем на `replicas:2`.
И повторно запустить `kubectl	apply -n netolgy deployment-nginx-multitool.yml`

![alt text](img/4.png)

Видно что стало два запущеных пода.

До этого было 1
![alt tetx](img/5.png)

После стало 2
![alt tetx](img/6.png)

Написан манифест Serivce [nginx-multitool-svc](/nginx-multitool-svc.yml)

Запуск данного манифеста

![alt tetx](img/7.png)

Проверка и видим что все создалось.

![alt text](img/8.png)

Написан манифест отдельного пода [multitool](/multitool.yml) и запускаю.

![alt text](img/9.png)

Проверка подов в namespace netology

![alt text](img/10.png)

Под поднялся.

С помощью `curl`, проверяю из пода multitool доступ до приложений из п.1:

Сначала на порт 80
![alt text](img/11.png)

Теперь на порт 8080
![alt tetx](img/12.png)

### Решение задания 2. Создать Deployment и обеспечить старт основного контейнера при выполнении условий

1. Создан манифест Deployment приложения nginx, который запустится только после запуска сервиса.
[nginx-init-deploy](nginx-init-deploy.yml)

![alt text](img/13.png)

Проверка запустился ли под

![alt text](img/14.png)

Создан манифест Service [nginx-init-svc](/nginx-init-svc.yml)
Запуск и проверка.
![alt text](img/15.png)
