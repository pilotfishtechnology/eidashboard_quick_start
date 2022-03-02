# Quick reference

-	**Maintained by**: [PilotFish Technology, LLC](https://www.pilotfishtechnology.com)

-	**Where to get help**: [PilotFish Technology CMS](https://cms.pilotfishtechnology.com)

-   **Docker Images**: [Docker Hub](https://hub.docker.com/u/pilotfishtechnology)

-   **Source**: [GitHub](https://github.com/pilotfishtechnology)

# What is eiDashboard?
PilotFish’s eiDashboard UI delivers multi-dimensional operational insight for greatly reduced downtime and issue resolution on the fly. Users get to view interface activity, from high-level message orchestrations all the way down to discrete operations. Developers gain access to fix issues on the fly. Business users are provided with the kind of operational detail that enables them to troubleshoot and resolve issues at a greater speed, and far more efficiently than was previously possible.

[healthcare.pilotfishtechnology.com/eidashboard/](https://healthcare.pilotfishtechnology.com/eidashboard/)

![logo](https://www.pilotfishtechnology.com/wp-content/uploads/2015/03/pilotfish-logo.png)

## Quick Setup

1. Install Docker

	- [Docker Install documentation](https://docs.docker.com/install/)

2. Clone the main branch of the [repository](https://github.com/pilotfishtechnology/eidashboard_quick_start)

	```bash
	cd /opt
	git clone https://github.com/pilotfishtechnology/eidashboard_quick_start
	cd eidashboard_quick_start
	```

3. Log in to the [Customer Portal](https://customerportal.pilotfishtechnology.com/portal/login.html) and download your latest license file.

4. Copy in your license file (`pflicense.key`).

5. Bring up your stack by running

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
	  -e PFISH_DASH_CONF_com_pilotfish_eip_txlog_dbUrl=jdbc:postgresql://postgres:5432/txnlogdb \
	  -e PFISH_DASH_CONF_com_pilotfish_eip_txlog_dbUser=${PSQL_USERNAME} \
	  -e PFISH_DASH_CONF_com_pilotfish_eip_txlog_dbPass=${PSQL_PASSWORD} \
	  -e PFISH_EIP_CONF_com_pilotfish_eip_user=${EIPLATFORM_USERNAME} \
	  -e PFISH_EIP_CONF_com_pilotfish_eip_password=${EIPLATFORM_PASSWORD} \
	  --network=eidashboard-network \
	  pilotfishtechnology/eidashboard:${EIDASHBOARD_VERSION} 
	```

When your docker container is running, connect to it on port `8080` to access the eiDashboard instance.

[http://127.0.0.1:8080/dashboard/](http://127.0.0.1:8080/dashboard/)

