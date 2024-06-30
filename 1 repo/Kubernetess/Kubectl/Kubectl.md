[[Kubernetes]]

- Средство командной строки для управления кластером
- Команды:
- [[Config file for Kubectl]]

- kubectl get nodes - показывает запущенные nodы
- kubectl get pod - показывает модули
- kubectl get pod -o wide -> показывает расширенное описание
- kubectl get services - показывает сервисы
- kubectl get deployment - показывает развертывани
- kubectl get replicaset - показывает все реплики
- kubectl get all - получить все 

kubectl create deployment <name> --image=<mage_name>-> создает развертывание с 
	именем <name> по определенному образу <image_name>
	
kubectl edit deployment <name> -> открывает файл с конфигурацией deployment а который 
	можно редактировать и pod будет пересоздан 
	
kubectl logs <fullname> - показывает логи внутри pod
	<fullname> - полное имя после команды get pod
	
kubectl describe pod <fullname> -> показывает полное состояние pod

kubectl exec -it <fullname> --bin/bash -> получаем доступ к теминалу контрейнера

kubectl delete deployment <name> -> удаляет развертывание и все реплики внутри

Для удобства лучше использовать файлы конфигурации
kubectl apply -f <filename> -> принимает все настройки описанные в данном файле (yaml)
к примеру nginx-deployment.yaml - в этом случае для deployment nginx