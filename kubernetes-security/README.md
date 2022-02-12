### Kubernetes Security Tasks

#### task01
1. Создать Service Account bob, дать ему роль admin в рамках всего кластера
2. Создать Service Account dave без доступа к кластеру

#### task02
1. Создать Namespace prometheus
2. Создать Service Account carol в этом Namespace
3. Дать всем Service Account в Namespace prometheus возможность делать get, list, watch в отношении Pods всего кластера

#### task03
1. Создать Namespace dev
2. Создать Service Account jane в Namespace dev
3. Дать jane роль admin в рамках Namespace dev
4. Создать Service Account ken в Namespace dev
5. Дать ken роль view в рамках Namespace dev