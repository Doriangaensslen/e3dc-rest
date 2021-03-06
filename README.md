# e3dc-rest
a simple REST API to access a E3DC system

## Required Configuration

```
export E3DC_IP_ADDRESS='192.168.1.99'
export E3DC_USERNAME='use@domain.com'
export E3DC_PASSWORD='Passw0rd'
export E3DC_KEY='secretkey'
export ADMIN_PASSWORD='admin'
```

## Start

### Local

```
python -m venv .venv
source .venv
pip install -r requirements.txt
```

Dev Webserver:
```
python api.py
```
or use
```
gunicorn --bind 0.0.0.0:8080 wsgi:app --access-logfile -
```

### Container

#### Build

```
docker build . -t e3dc-rest
```

Multi Arch Images are available with `vchrisb/e3dc-rest` here https://hub.docker.com/repository/docker/vchrisb/e3dc-rest

#### Docker

```
docker run --name e3dc-rest -p 8080:8080 -e E3DC_IP_ADDRESS -e E3DC_USERNAME -e E3DC_PASSWORD -e E3DC_KEY -e ADMIN_PASSWORD="admin" e3dc-rest
```

#### Kubernetes

Create Secret:
```
kubectl create secret generic e3dc-secret --from-literal=username='user@domain.com' --from-literal=password='password' --from-literal=ip_address='192.168.1.99' --from-literal=key='password' --from-literal=admin_password='admin'
```

Deploy with Ingress Controller:
```
kubectl apply -f ingress.yml -f service-ingress.yml -f deplyoment.yml
```
## Use

```
curl http://admin:admin@localhost:8080/api/poll
curl -H "Content-Type: application/json"  -X POST -d '{"powerLimitsUsed": true,"maxChargePower": 1000,"maxDischargePower": 1300,"dischargeStartPower": 65}' http://admin:admin@127.0.0.1:8080/api/power_settings
curl http://admin:admin@localhost:8080/api/power_settings 
```