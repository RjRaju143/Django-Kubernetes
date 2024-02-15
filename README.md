# Django & Kubernetes

## Cloning this Repo?

Make sure you create `web/.env` and fill in the following variables:

```
DEBUG=1
REGION=texas
DJANGO_SUPERUSER_USERNAME=admin
DJANGO_SUPERUSER_PASSWORD=mydjangopw
DJANGO_SUERPUSER_EMAIL=hello@teamcfe.com
DJANGO_SECRET_KEY=fix_this_later

POSTGRES_READY=0
POSTGRES_DB=dockerdc
POSTGRES_PASSWORD=mysecretpassword
POSTGRES_USER=myuser
POSTGRES_HOST=localhost
POSTGRES_PORT=5434

REDIS_HOST=redis_db
REDIS_PORT=6388
```
> If you change `POSTGRES_PORT` or `REDIS_PORT` be sure to update those values in `docker-compose.yaml`

Once you have the above `.env` file, navigate to your project root (right where `docker-compose.yaml` is) and run:

```
docker compose up -d
```
This will create a `postgresql` database that's running in the background for you. To bring this database down just run:

```
docker compose down
```
The data in the database will be persistent so you can run `docker compose up -d` again with confidence. 


4. Update secrets (if needed)

kubectl delete secret django-k8s-web-prod-env
kubectl create secret generic django-k8s-web-prod-env --from-env-file=web/.env.prod

