FROM python:3.7
COPY templates ./templates
COPY mysql-operator.py ./mysql-operator.py
RUN pip install kopf kubernetes pyyaml==5.4.1 jinja2
CMD kopf run /mysql-operator.py