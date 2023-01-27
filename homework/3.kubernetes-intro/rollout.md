#Создадим неймспейс:
kubectl create namespace kubedoom

#Установим его как текущий:
kubectl config set-context --current --namespace=kubedoom

#Применяем манифест
kubectl apply -f manifest.yml

#Применяем деплой
kubectl apply -f kubedoom.yml

#Контролируем работу
kubectl get deploy
kubectl get pods

#Применяем LoadBalancer для перенаправления портов
kubectl apply -f loadbalancer.yml

#Проверяем работу и получаем IP
kubectl get svc

#Запускаем VNC-клиент
vncviewer

#Вводим полученный IP и порт 8888
#Вводим пароль

