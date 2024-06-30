[[Описание конфиг файла]]

- Содержит конфигурацию
```json
apiVersion: v1
kind: ConfigMap 

metadata:
  name: postgres-config
data:
  databaseUri: postgres-serv
```
- Применение
```json
valueFrom:
	configMapKeyRef:
		name: postgres-config
		key: databaseUri
```