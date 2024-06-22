Primeiro build: --- OK
docker build \
-f src/services/NotSoSimpleEcommerce.Main/Dockerfile \
-t nsse/main:1.0.0 \
.

---
trivy image nsse/main:1.0.0
trivy config src/services/NotSoSimpleEcommerce.Main/Dockerfile 
---

Não está OK. Ver o comando certo na NOTA abaixo
docker run --env-file .env -p 8080:443 \
-v ./certificates/kestrel-certificate.pfx:/certificates/kestrel-certificate.pfx:ro \
nsse/main:1.0.0

docker run --rm -it --env-file .env -v ./certificates/kestrel-certificate.pfx:/certificates/kestrel-certificate.pfx:ro nsse/main:1.0.0 /bin/bash
ls -l /certificates/kestrel-certificate.pfx

NOTA: O comando da aula não funciona, pois provavelmente o fonte Program.cs estava trabalhando sem o mongo
docker run --env-file ./.env -p 8080:443 \
  -v ./certificates/kestrel-certificate.pfx:/certificates/kestrel-certificate.pfx:ro \
  -v ./certificates/mongo-ca.pem:/certificates/mongo-ca.pem:ro \
  nsse/main:1.0.0

  Verifique com: docker container ls
CONTAINER ID   IMAGE                  COMMAND                  CREATED          STATUS                    PORTS                                                                 NAMES
df28ed8f7599   nsse/main:1.0.0        "dotnet /nsse-backen…"   51 seconds ago   Up 49 seconds (healthy)   0.0.0.0:8080->443/tcp              
    funny_diffie

docker exec -it df28ed8f7599 /bin/sh
/nsse-backend $ id
uid=1000(dotnet) gid=1000(dotnet) groups=1000(dotnet)
/nsse-backend

docker inspect df28ed8f7599

nota: o -k permite conexões inseguras
curl -k --fail https://localhost:443/main/health
https://localhost:8080/main/swagger/index.html







