## Kubernetes Controllers

### Frontend Replicaset

1. Добавлен манифест типа ReplicaSet для frontend app
2. Применение ReplicaSet с новой ревизией образа и обновление подов не произошло из-за того, что предыдущий манифест ReplicaSet со старым на текущий момент образом уже был успешно применен и количество подов уже соответствовало желаемому (отфильтрованным по лейблу app=frontend). При удаление старых подов новый манифест ReplicaSet успешно применился

### PaymentService Deployment

1. Добавлен манифест типа Deployment для paymentservice app
2. Решены дополнительные задачи со * и добавлены манифесты :
   - BG deployment с конфигурацией (maxSurge: 100%, maxUnavailable: 0)
   - Reverse RollingUpdate с конфигурацией (maxSurge: 0, maxUnavailable: 1)

### Frontend Deployment

1. Добавлен манифест типа Deployment для frontend app
2. Добавлена readinessProbe для проверки готовности подов frontend

### Node-Exporter DaemonSet

1. Добавлен манифест типа DaemonSet для node-exporter
2. Для запуска node-exporter на master ноде добавлен следующий блок:
```yaml
spec:
  tolerations:
  - key: node-role.kubernetes.io/master
```


