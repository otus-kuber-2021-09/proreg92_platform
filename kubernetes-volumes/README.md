### Kubernetes Volumes

1. Worked with minio and configured open data of minio to kubernetes secrets

E.g.: minio-statefulset-secrets.yaml, minio-secrets.yaml

#### Checking from pod:

```sh
$ k exec -ti minio-0 /bin/sh

/ # echo "$MINIO_ACCESS_KEY"
minio
/ # echo "$MINIO_SECRET_KEY"
minio123
/ # exit
```

2. Also checking from mc pod in kind cluster: minio-mc-pod.yaml

```sh
$ k exec -ti minioclient /bin/sh
$ mc ls
$ mc mc alias set myminio http://10.244.0.7:9000 $MINIO_ACCESS_KEY $MINIO_SECRET_KEY
$ mc cp /tmp/file123 myminio/test/file123
$ sh-4.2$ mc cp file123 myminio/test/file123
    file123:                               18 B / 18 B ┃▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓┃ 100.00% 9.54 KiB/s 0s

$ sh-4.2$ mc cat myminio/test/file123
    12121
    333

    hello!
```
