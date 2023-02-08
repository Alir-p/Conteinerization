#Создадим неймспейс:
❯ kubectl create namespace gb4

#Установим его как текущий:
❯ kubectl config set-context --current --namespace=gb4

#Применяем деплой:
❯ kubectl create -f 4.resources-and-persistence
deployment.apps/postgres created
persistentvolumeclaim/pgdata created
secret/pgpassword created

##Проверка работоспособности
#Получаем IP пода postgresql:
❯ kubectl get po -o wide 
NAME                        READY   STATUS    RESTARTS   AGE    IP               NODE 
postgres-64d5d4bc98-wsfdr   1/1     Running   0          104s   10.100.248.128   gb-kub-master-gb-kub-node-1

#Запускаем рядом тестовый под:
❯ kubectl run -t -i --rm --image postgres:10.13 test bash
If you don't see a command prompt, try pressing enter.
root@test:/#

#Внутри тестового пода подключаемся к БД:
root@test:/# psql -h 10.100.248.128 -U testuser testdatabase
Password for user testuser: 
psql (10.13 (Debian 10.13-1.pgdg90+1))
testdatabase=#

#Создаем таблицу:
testdatabase=# CREATE TABLE testtable (testcolumn VARCHAR (50) );
CREATE TABLE

#Проверяем наличие таблицы:
testdatabase=# \dt
           List of relations
 Schema |   Name    | Type  |  Owner 
--------+-----------+-------+----------
 public | testtable | table | testuser
(1 row)

#Выйдим из тестового пода, удаляем под с postgresql:

testdatabase=# \q
root@test:/# exit
exit
Session ended, resume using 'kubectl attach test -c test -i -t' command when the pod is running
pod "test" deleted
❯ kubectl delete pod/postgres-64d5d4bc98-wsfdr
pod "postgres-64d5d4bc98-wsfdr" deleted
❯ kubectl get po
NAME                        READY   STATUS              RESTARTS   AGE
postgres-64d5d4bc98-8ntww   0/1     ContainerCreating   0          10s

#После его пересоздания проверяем что созданная ранее таблица никуда не делась:
❯ kubectl get po -o wide 
NAME                        READY   STATUS    RESTARTS   AGE   IP              NODE
postgres-64d5d4bc98-8ntww   1/1     Running   0          61s   10.100.121.65   gb-kub-master-gb-kub-node-0 
❯ kubectl run -t -i --rm --image postgres:10.13 test bash
If you don't see a command prompt, try pressing enter.
root@test:/# psql -h 10.100.121.65 -U testuser testdatabase
Password for user testuser: 
psql (10.13 (Debian 10.13-1.pgdg90+1))
Type "help" for help.

testdatabase=# \dt
           List of relations
 Schema |   Name    | Type  |  Owner 
--------+-----------+-------+----------
 public | testtable | table | testuser
(1 row)

testdatabase=# \q
root@test:/# exit
exit
Session ended, resume using 'kubectl attach test -c test -i -t' command when the pod is running
pod "test" deleted
❯ 
