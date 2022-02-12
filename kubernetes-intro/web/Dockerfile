FROM python:latest
LABEL Sergey <s.zolotov92@gmail.com>

ARG PORT=8000

USER 1001:1001
WORKDIR /app
ADD index.html /app
CMD python -m http.server 8000