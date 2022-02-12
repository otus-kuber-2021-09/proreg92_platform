# pip3/python modules

```sh
pip3 install pyyaml==5.4.1
pip3 install kopf jinja2 kubernetes
```

### checking db

```sh
export MYSQLPOD=$(kubectl get pods -l app=mysql-instance -o jsonpath="{.items[*].metadata.name}")
    
kubectl exec -it $MYSQLPOD -- mysql -u root  -potuspassword -e "CREATE TABLE test ( id smallint unsigned not null auto_increment name varchar(20) not null, constraint pk_example primary key (id) );" otus-database

kubectl exec -it $MYSQLPOD -- mysql -potuspassword  -e "INSERT INTO test ( id, name ) VALUES ( null, 'some data' );" otus-database
     
kubectl exec -it $MYSQLPOD -- mysql -potuspassword -e "INSERT INTO test ( id, name ) VALUES ( null, 'some data-2' );" otus-database

kubectl exec -it $MYSQLPOD -- mysql -potuspassword -e "select * from test;" otus-database
```

### Docker hub: mysql-operator

link: https://hub.docker.com/layers/192109157/proreg92/otus_crd_mysqloperator/1.0/images/sha256-cfad37312e90ca1fa6c77375ffd0ceaaca732efa93455ffef40609c1fcc28672?context=repo 