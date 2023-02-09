#Создадим неймспейс:
❯ kubectl create namespace gb5

#Установим его как текущий:
❯ kubectl config set-context --current --namespace=gb5

#Применяем деплой:
❯ kubectl create -f .

##Проверка работоспособности
#Получаем IP сервиса ingress-nginx-controller:
❯ kubectl get svc -A

#Переходим по полученному адресу в браузере:
Наблюдаем домашнюю страницу Redmine


