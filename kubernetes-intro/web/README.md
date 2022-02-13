# Выполнено ДЗ № 1

 - [x] HW1 Task 1: Dockerfile with Simple web server

## В процессе сделано:
 - Dockerfile with simple web server and page
 - Star Page

## Как запустить проект:
 - docker pull proreg92/otus_task1
 - docker run -p 8000:8000 --rm -it proreg92/otus_task1:1.0

## Как проверить работоспособность:
 - After executing cmd 'docker run...' ,  go to in your browser and type http://localhost:8000

## Инструкция по работе с Докер и Докер Хабом:
 - build: docker build . -t proreg92/otus_task1:1.0 --no-cache
 - run: docker run -p 8000:8000 --rm -it proreg92/otus_task1:1.0
 - tag: docker tag otus_task1:1.0 proreg92/otus_task1:1.0
 - push: docker push proreg92/otus_task1:1.0

### !!! Прежде всего нужно создать Access_Tokens на GitHub и DockerHub и залогиниться
 - docker login -u proreg92



