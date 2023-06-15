## Тестовое задание Алексей Гайдуков

**Докер-образ**
 - Доступен для скачивания из публичного репозитория gino20d/test_task
 - Dockerfile и необходимые конфигурационные файлы находятся в корневой папке репозитория github.com

**Манифесты kubernetes**
Находятся в папке manifest

**Helm чарты**
Находятся в папке helm

**CI-CD скрипт**
Содержится в файле .gitlab-ci.yml

**Проверка работоспособности сервиса в кластере**

 - В манифесте сервиса добавлен startupProbe, поздразумевающий обращение по адресу http://:8000/test.html. В сочетании с деплоем сервиса с применением helm с флагом --atomic ошибка при разворачивании в кластере приведет к ошибке пайплайна и откату состояния кластера.
 - Дополнительно в пайплайн включена job, использующая встроенный механизм helm для проверки работоспособности сервиса. В ходе её выполнения в кластер деплоится отдельный под, который опрашивает наш сервис. В случае ошибки указанная job также завершится ошибкой.
 - В манифесте включена проверка сервиса с использованием livenessProbe. Если в какой-то момент сервис не сможет отдать ответ с кодом 200 по адресу  http://:8000/test.html, он будет перезапущен.
 - Выполнить команду helm list и убедиться в успешной установке пакета.
 - Можно проверить текущий статус сервиса по логам контейнера (kubectl logs deployment/test-task) и статусу (kubectl describe deployment/test-task).
 - Запустить утилиту k9s и посмотреть состояние сервиса в реальном времени.

**Удаление и восстановление системных подов**

 - kube-apiserver является статическим подом, состояние которого отслеживатся непосредственно kubelet. Это также следует из пункта Controlled By описания пода (kubectl describe po/kube-apiserver-minikube -n kube-system)
 - core-dns контролируется абстракцией ReplicaSet, механизм которой отслеживает необходимое количество реплик сервиса для достижения требуемого состояния.

