# Start

```bash
EIDASHBOARD_VERSION=22R1
PSQL_USERNAME=postgres
PSQL_PASSWORD=postgres
EIDASHBOARD_PORT=8080
EIPLATFORM_USERNAME=eiplatform
EIPLATFORM_PASSWORD=eiplatform
  
docker network create eidashboard-network

docker run -it -d --rm --name postgres \
  -v $(pwd)/postgres-data:/var/lib/postgresql/data \
  -e POSTGRES_USER=${PSQL_USERNAME} \
  -e POSTGRES_PASSWORD=${PSQL_PASSWORD} \
  -e POSTGRES_DB=txnlogdb \
  --network=eidashboard-network \
  pilotfishtechnology/postgres:${EIDASHBOARD_VERSION} 

docker run -it -d --rm --name eidashboard \
  -v $(pwd)/pflicense.key:/opt/pilotfish/license/pflicense.key \
  -p ${EIDASHBOARD_PORT}:8080 \
  -e PFISH_dashboard_txlogapi_url=http://localhost:8080/eip/rest \
  -e PFISH_DASH_CONF_com_pilotfish_eip_txlog_dbUrl=jdbc:postgresql://db:5432/txnlogdb \
  -e PFISH_DASH_CONF_com_pilotfish_eip_txlog_dbUser=${PSQL_USERNAME} \
  -e PFISH_DASH_CONF_com_pilotfish_eip_txlog_dbPass=${PSQL_PASSWORD} \
  -e PFISH_EIP_CONF_com_pilotfish_eip_user=${EIPLATFORM_USERNAME} \
  -e PFISH_EIP_CONF_com_pilotfish_eip_password=${EIPLATFORM_PASSWORD} \
  --network=eidashboard-network \
  pilotfishtechnology/eidashboard:${EIDASHBOARD_VERSION} 
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

EIDASHBOARD_VERSION=22R1
PSQL_USERNAME=postgres
PSQL_PASSWORD=postgres
EIDASHBOARD_PORT=8080
EIPLATFORM_USERNAME=eiplatform
EIPLATFORM_PASSWORD=eiplatform

docker stop postgres
docker stop eidashboard

docker pull pilotfishtechnology/postgres:${EIDASHBOARD_VERSION} 
docker pull pilotfishtechnology/eidashboard:${EIDASHBOARD_VERSION} 

docker run -it -d --rm --name postgres \
  -v $(pwd)/postgres-data:/var/lib/postgresql/data \
  -e POSTGRES_USER=${PSQL_USERNAME} \
  -e POSTGRES_PASSWORD=${PSQL_PASSWORD} \
  -e POSTGRES_DB=txnlogdb \
  --network=eidashboard-network \
  pilotfishtechnology/postgres:${EIDASHBOARD_VERSION} 

docker run -it -d --rm --name eidashboard \
  -v $(pwd)/pflicense.key:/opt/pilotfish/license/pflicense.key \
  -p ${EIDASHBOARD_PORT}:8080 \
  -e PFISH_dashboard_txlogapi_url=http://localhost:8080/eip/rest \
  -e PFISH_DASH_CONF_com_pilotfish_eip_txlog_dbUrl=jdbc:postgresql://db:5432/txnlogdb \
  -e PFISH_DASH_CONF_com_pilotfish_eip_txlog_dbUser=${PSQL_USERNAME} \
  -e PFISH_DASH_CONF_com_pilotfish_eip_txlog_dbPass=${PSQL_PASSWORD} \
  -e PFISH_EIP_CONF_com_pilotfish_eip_user=${EIPLATFORM_USERNAME} \
  -e PFISH_EIP_CONF_com_pilotfish_eip_password=${EIPLATFORM_PASSWORD} \
  --network=eidashboard-network \
  pilotfishtechnology/eidashboard:${EIDASHBOARD_VERSION} 

```