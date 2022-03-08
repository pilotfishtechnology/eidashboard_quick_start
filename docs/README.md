# Start

```bash
docker run -it -d --name postgres \
  -v $(pwd)/db-data:/var/lib/postgresql/data \
  --env-file ./.env \
  --network=eidashboard-network \
  pilotfishtechnology/postgres:latest

docker run -it -d --name eidashboard \
  -v $(pwd)/logs:/opt/pilotfish/logs \
  -v $(pwd)/license:/opt/pilotfish/license \
  -v $(pwd)/config:/opt/pilotfish/config \
  -p 8080:8080 \
  --env-file ./.env \
  --network=eidashboard-network \
  pilotfishtechnology/eidashboard:latest
```

# Stop

```bash
docker stop postgres
docker stop eidashboard
```

# View Logs

```bash
docker logs -f eidashboard
```

# Upgrade

```bash
docker stop postgres
docker stop eidashboard

docker rm postgres
docker rm eidashboard

docker pull pilotfishtechnology/postgres:latest
docker pull pilotfishtechnology/eidashboard:latest

docker run -it -d --name postgres \
  -v $(pwd)/db-data:/var/lib/postgresql/data \
  --env-file ./.env \
  --network=eidashboard-network \
  pilotfishtechnology/postgres:latest

docker run -it -d --name eidashboard \
  -v $(pwd)/logs:/opt/pilotfish/logs \
  -v $(pwd)/license:/opt/pilotfish/license \
  -v $(pwd)/config:/opt/pilotfish/config \
  -p 8080:8080 \
  --env-file ./.env \
  --network=eidashboard-network \
  pilotfishtechnology/eidashboard:latest
```