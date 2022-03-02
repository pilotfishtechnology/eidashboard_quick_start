# Start

```bash
docker run -it -d --rm --name postgres \
  -v $(pwd)/postgres-data:/var/lib/postgresql/data \
  --env-file ./.env \
  --network=eidashboard-network \
  pilotfishtechnology/postgres:22R1

docker run -it -d --rm --name eidashboard \
  -v $(pwd)/pflicense.key:/opt/pilotfish/license/pflicense.key \
  -p 8080:8080 \
  --env-file ./.env \
  --network=eidashboard-network \
  pilotfishtechnology/eidashboard:22R1
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

docker pull pilotfishtechnology/postgres:22R1
docker pull pilotfishtechnology/eidashboard:22R1

docker run -it -d --rm --name postgres \
  -v $(pwd)/postgres-data:/var/lib/postgresql/data \
  --env-file ./.env \
  --network=eidashboard-network \
  pilotfishtechnology/postgres:22R1

docker run -it -d --rm --name eidashboard \
  -v $(pwd)/pflicense.key:/opt/pilotfish/license/pflicense.key \
  -p 8080:8080 \
  --env-file ./.env \
  --network=eidashboard-network \
  pilotfishtechnology/eidashboard:22R1
```