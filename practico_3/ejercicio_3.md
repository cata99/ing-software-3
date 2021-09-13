cata@cata-UX550VD:~$ sudo docker network create -d bridge mybridge 
046bae8dd7726371bd0bba8b678ba5d6fb7ca03751ee73efef3253b8f2d2131d


cata@cata-UX550VD:~$ sudo docker run -d --net mybridge --name db redis:alpine
3167449626b1372158e43a8a36aac591eb6543f4abd50ab61565695b3080e0b9


cata@cata-UX550VD:~$ sudo docker run -d --net mybridge -e REDIS_HOST=db -e REDIS_PORT=6379 -p 		5000:5000 --name web alexisfr/flask-app:latest
f1245dd492f66b93ff2ff93f737f6d04b9b023a87efd429ff16c52eca71b8e25

cata@cata-UX550VD:~/Documents/Segundo Semestre$ sudo docker network ls
NETWORK ID     NAME                            DRIVER    SCOPE
046bae8dd772   mybridge                        bridge    local
cata@cata-UX550VD:~/Documents/Segundo Semestre$ sudo docker network inspect mybridge
[
    {
        "Name": "mybridge",
        "Id": "046bae8dd7726371bd0bba8b678ba5d6fb7ca03751ee73efef3253b8f2d2131d",
        "Created": "2021-08-30T18:58:42.325355674-03:00",
        "Scope": "local",
        "Driver": "bridge",
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": {},
            "Config": [
                {
                    "Subnet": "172.21.0.0/16",
                    "Gateway": "172.21.0.1"
                }
            ]
        },
        "Internal": false,
        "Attachable": false,
        "Ingress": false,
        "ConfigFrom": {
            "Network": ""
        },
        "ConfigOnly": false,
        "Containers": {
            "3167449626b1372158e43a8a36aac591eb6543f4abd50ab61565695b3080e0b9": {
                "Name": "db",
                "EndpointID": "f6c84cbc7bfb6aac5523ad19154fc7e89088bb548aef546998e99812f7f45941",
                "MacAddress": "02:42:ac:15:00:02",
                "IPv4Address": "172.21.0.2/16",
                "IPv6Address": ""
            },
            "f1245dd492f66b93ff2ff93f737f6d04b9b023a87efd429ff16c52eca71b8e25": {
                "Name": "web",
                "EndpointID": "52ebe506b519f17b3ab019fddca1cc8e466d99dc45f5118ac31cfabb7319871e",
                "MacAddress": "02:42:ac:15:00:03",
                "IPv4Address": "172.21.0.3/16",
                "IPv6Address": ""
            }
        },
        "Options": {},
        "Labels": {}
    }
]

cata@cata-UX550VD:~/Documents/Segundo Semestre$ sudo docker ps 
CONTAINER ID   IMAGE                       COMMAND                  CREATED         STATUS         PORTS                                       NAMES
f1245dd492f6   alexisfr/flask-app:latest   "python /app.py"         7 minutes ago   Up 7 minutes   0.0.0.0:5000->5000/tcp, :::5000->5000/tcp   web
3167449626b1   redis:alpine                "docker-entrypoint.s…"   9 minutes ago   Up 9 minutes   6379/tcp        

2- ANALISIS DEL SISTEMA
- El sistema corre en el host 0.0.0.0 y cada vez que se accede al mismo muestra el mensaje "Helo from Redis! I have been seen (la cantidad de veces que se entro a la pagina) times "
- Los parametros -e se utilizan para setear las variables de entorno

cata@cata-UX550VD:~/Documents/Segundo Semestre$ sudo docker rm -f web
web

cata@cata-UX550VD:~/Documents/Segundo Semestre$ sudo docker run -d --net mybridge -e REDIS_HOST=db -e REDIS_PORT=6379 -p 5000:5000 --name web alexisfr/flask-app:latest
f14037287b5c4cd6e07b7d86803a2c10e032a5416e1124259416d3261dbad76e

- Si se realiza el rm y luego se lo vuelve a correr, se muestra el mensaje con la cantidad de veces ingresadas previas, a diferencia de si se borra el db ya que almacena la cantidad de veces que se ingreso

3- UTILIZANDO DOCKER COMPOSE 
cata@cata-UX550VD:~/Documents/Segundo Semestre$ sudo docker-compose up -d
Creating network "segundosemestre_default" with the default driver
Creating volume "segundosemestre_redis_data" with default driver
Creating segundosemestre_db_1 ... done
Creating segundosemestre_app_1 ... done

cata@cata-UX550VD:~/Documents/Segundo Semestre$ sudo docker ps
CONTAINER ID   IMAGE                       COMMAND                  CREATED         STATUS         PORTS                                       NAMES
5ae5a149afd7   alexisfr/flask-app:latest   "python /app.py"         2 minutes ago   Up 2 minutes   0.0.0.0:5000->5000/tcp, :::5000->5000/tcp   segundosemestre_app_1
1a1e41cb4351   redis:alpine                "docker-entrypoint.s…"   2 minutes ago   Up 2 minutes   6379/tcp                                    segundosemestre_db_1

cata@cata-UX550VD:~/Documents/Segundo Semestre$ sudo docker network ls 
NETWORK ID     NAME                            DRIVER    SCOPE
1bbb69780ed4   bridge                          bridge    local
2093d2937af8   example-voting-app_back-tier    bridge    local
cecd480ee666   example-voting-app_front-tier   bridge    local
1771220c1914   host                            host      local
046bae8dd772   mybridge                        bridge    local
c650d6c1c5ee   none                            null      local
056c07d7e240   segundosemestre_default         bridge    local


cata@cata-UX550VD:~/Documents/Segundo Semestre$ sudo docker volume ls 
DRIVER    VOLUME NAME
local     4ddfde1c936a10febadc5941b4a9c4ee9a89e5bbcff7e249f6e768c92b5132c6
local     6a69485a8bfbe8d3180c8b89f87aea804bbb29629d2822ee9492f388597e9faf
local     71cdb6846d696b60ba33c32552fb78ebbfef29406fbb8de8d223a7ac3c61593e
local     216fc9b722a3c46fe7d47d4d122440e610d5bd5436b78980ebab97aa8800ecdd
local     bfa3d8327b2ab06fe6d74106aaaf4b7f754865748ef63538ae6480728a300475
local     d30943f9013a54b38209c566811ecfe4711dc9b8a09972e2861de811177d3416
local     e5b4298735821e548b01a88cbb9dacf2e443d99feb42d336a8c7351a8d072458
local     example-voting-app_db-data
local     ing_software_iii_redis_data
local     segundosemestre_redis_data

- El docker compose se encargo de interpretar el archivo .yaml y ejecutarlo

cata@cata-UX550VD:~/Documents/Segundo Semestre$ sudo docker-compose down 
Stopping segundosemestre_app_1 ... done
Stopping segundosemestre_db_1  ... done
Removing segundosemestre_app_1 ... done
Removing segundosemestre_db_1  ... done
Removing network segundosemestre_default

4- AUMENTANDO LA COMPLEJIDAD, ANALISIS DE OTRO SISTEMA DISTRIBUIDO 

cata@cata-UX550VD:~/Documents/Segundo Semestre/Ing_Software_III/example-voting-app$ sudo docker-compose -f docker-compose-javaworker.yml up -d
Creating example-voting-app_worker_1 ... done
Creating example-voting-app_vote_1   ... done
Creating db                          ... done
Creating example-voting-app_result_1 ... done
Creating redis                       ... done





