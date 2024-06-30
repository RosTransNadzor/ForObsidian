[[Kubectl]]

- Описывает deployment,service ...  в файле yml
- Это упрощает конфигурацию элементов kubernetes
- Пример конфиг файла
- [[Описание конфиг файла]]
```json
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-dep
// описывает сам deployment
spec:
  replicas: 1
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        editor: vscode
        app: nginx
    // описывает контейнеры
    spec:
      containers:
      - name: nginx
        image: nginx
        ports:
        - containerPort: 80
        // переменные окружения
        env:
	    - name: SOMEVAR
		  value: 123
```
- Приминение и удаление конфиг файла
- kubectl apply -f <-filename->
- kubectl delete -f <-filename->
