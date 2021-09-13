1- INSTANCIACION DEL SISTEMA 
cata@cata-UX550VD:~/Documents/Segundo Semestre/Ing_Software_III/socks-demo/microservices-demo$ sudo docker-compose -f deploy/docker-compose/docker-compose.yml up -d
[sudo] password for cata: 
WARNING: The MYSQL_ROOT_PASSWORD variable is not set. Defaulting to a blank string.
Creating network "docker-compose_default" with the default driver
Pulling front-end (weaveworksdemos/front-end:0.3.12)...
0.3.12: Pulling from weaveworksdemos/front-end
709515475419: Pulling fs layer
709515475419: Downloading [>                                                  ]  23.89kB/2.313MBwnloading [>                                                  ] 8a7a0a7b6b80: Downloading [>                                                  ] 709515475419: Downloading [====>                                              ] 709515475419: Pull complete
8a7a0a7b6b80: Pull complete
79dadb976eeb: Pull complete
f48430971a7c: Pull complete
69ee1f19577a: Pull complete
89bb0a032ada: Pull complete
01d99281a40a: Pull complete
07a3ae088a4f: Pull complete
a9efb8659a27: Pull complete
Digest: sha256:26a2d9b6b291dee2dca32fca3f5bff6c2fa07bb5954359afcbc8001cc70eac71
Status: Downloaded newer image for weaveworksdemos/front-end:0.3.12
Pulling edge-router (weaveworksdemos/edge-router:0.1.1)...
0.1.1: Pulling from weaveworksdemos/edge-router
b7f33cc0b48e: Already exists

cata@cata-UX550VD:~/Documents/Segundo Semestre/Ing_Software_III/socks-demo/microservices-demo$ sudo docker ps 
CONTAINER ID   IMAGE                                COMMAND                  CREATED          STATUS          PORTS                                                                              NAMES
02d4e68b7d28   weaveworksdemos/catalogue-db:0.3.0   "docker-entrypoint.s…"   5 minutes ago    Up 5 minutes    3306/tcp                                                                           docker-compose_catalogue-db_1
8d41774f9c1a   weaveworksdemos/catalogue:0.3.5      "/app -port=80"          5 minutes ago    Up 5 minutes    80/tcp                                                                             docker-compose_catalogue_1
88ace3d7dfac   weaveworksdemos/orders:0.4.7         "/usr/local/bin/java…"   5 minutes ago    Up 5 minutes                                                                                       docker-compose_orders_1
7d02ee6e8009   weaveworksdemos/carts:0.4.8          "/usr/local/bin/java…"   5 minutes ago    Up 5 minutes                                                                                       docker-compose_carts_1
21fd8b535c7a   weaveworksdemos/shipping:0.4.8       "/usr/local/bin/java…"   5 minutes ago    Up 5 minutes                                                                                       docker-compose_shipping_1
37971456bd74   weaveworksdemos/edge-router:0.1.1    "traefik"                5 minutes ago    Up 5 minutes    0.0.0.0:80->80/tcp, :::80->80/tcp, 0.0.0.0:8080->8080/tcp, :::8080->8080/tcp       docker-compose_edge-router_1
4b41493e2f7f   weaveworksdemos/payment:0.4.3        "/app -port=80"          5 minutes ago    Up 5 minutes    80/tcp                                                                             docker-compose_payment_1
2529ee63cf7b   weaveworksdemos/user:0.4.4           "/user -port=80"         5 minutes ago    Up 5 minutes    80/tcp                                                                             docker-compose_user_1
6936f0621323   mongo:3.4                            "docker-entrypoint.s…"   5 minutes ago    Up 5 minutes    27017/tcp                                                                          docker-compose_carts-db_1
4df98a805d43   weaveworksdemos/user-db:0.4.0        "/entrypoint.sh mong…"   5 minutes ago    Up 5 minutes    27017/tcp                                                                          docker-compose_user-db_1
5b7b371b22ef   rabbitmq:3.6.8                       "docker-entrypoint.s…"   5 minutes ago    Up 5 minutes    4369/tcp, 5671-5672/tcp, 25672/tcp                                                 docker-compose_rabbitmq_1
4aabc10ee8ae   weaveworksdemos/front-end:0.3.12     "/usr/local/bin/npm …"   5 minutes ago    Up 5 minutes    8079/tcp                                                                           docker-compose_front-end_1
cef4f8881ef0   mongo:3.4                            "docker-entrypoint.s…"   5 minutes ago    Up 5 minutes    27017/tcp                                                                          docker-compose_orders-db_1
3a04462582d7   weaveworksdemos/queue-master:0.3.1   "/usr/local/bin/java…"   5 minutes ago    Up 5 minutes                                                                                       docker-compose_queue-master_1
c72afa3fb7f4   redis:alpine                         "docker-entrypoint.s…"   25 minutes ago   Up 25 minutes   0.0.0.0:49154->6379/tcp, :::49154->6379/tcp                                        redis
cc799f90c166   postgres:9.4                         "docker-entrypoint.s…"   25 minutes ago   Up 25 minutes   5432/tcp                                                                           db
37585fac9176   example-voting-app_vote              "python app.py"          25 minutes ago   Up 25 minutes   0.0.0.0:5000->80/tcp, :::5000->80/tcp                                              example-voting-app_vote_1
09754038fd64   example-voting-app_result            "docker-entrypoint.s…"   25 minutes ago   Up 25 minutes   0.0.0.0:5858->5858/tcp, :::5858->5858/tcp, 0.0.0.0:5001->80/tcp, :::5001->80/tcp   example-voting-app_result_1
17a8cc1c2db5   example-voting-app_worker            "java -XX:+UnlockExp…"   25 minutes ago   Up 25 minutes                                                                                      example-voting-app_worker_1


2- Investigación de componentes 
CLONAR LOS REPOS 
cata@cata-UX550VD:~/Documents/Segundo Semestre/Ing_Software_III/socks-demo$ git clone https://github.com/microservices-demo/front-end.git
Cloning into 'front-end'...
remote: Enumerating objects: 1228, done.
remote: Total 1228 (delta 0), reused 0 (delta 0), pack-reused 1228
Receiving objects: 100% (1228/1228), 47.89 MiB | 1.07 MiB/s, done.
Resolving deltas: 100% (683/683), done.
cata@cata-UX550VD:~/Documents/Segundo Semestre/Ing_Software_III/socks-demo$ git clone https://github.com/microservices-demo/user.git
Cloning into 'user'...
remote: Enumerating objects: 1063, done.
remote: Total 1063 (delta 0), reused 0 (delta 0), pack-reused 1063
Receiving objects: 100% (1063/1063), 172.83 KiB | 926.00 KiB/s, done.
Resolving deltas: 100% (601/601), done.
cata@cata-UX550VD:~/Documents/Segundo Semestre/Ing_Software_III/socks-demo$ git clone https://github.com/microservices-demo/edge-router.git
Cloning into 'edge-router'...
remote: Enumerating objects: 50, done.
remote: Total 50 (delta 0), reused 0 (delta 0), pack-reused 50
Unpacking objects: 100% (50/50), 14.49 KiB | 436.00 KiB/s, done.

5. cata@cata-UX550VD:~/Documents/Segundo Semestre/Ing_Software_III/socks-demo$ {"_embedded":{"customer":[{"firstName":"Eve","lastName":"Berger","username":"Eve_Berger","id":"57a98d98e4b00679b4a830af","_links":{"addresses":{"href":"http://user/customers/57a98d98e4b00679b4a830af/addresses"},"cards":{"href":"http://user/customers/57a98d98e4b00679b4a830af/cards"},"customer":{"href":"http://user/customers/57a98d98e4b00679b4a830af"},"self":{"href":"http://user/customers/57a98d98e4b00679b4a830af"}}},{"firstName":"User","lastName":"Name","username":"user","id":"57a98d98e4b00679b4a830b2","_links":{"addresses":{"href":"http://user/customers/57a98d98e4b00679b4a830b2/addresses"},"cards":{"href":"http://user/customers/57a98d98e4b00679b4a830b2/cards"},"customer":{"href":"http://user/customers/57a98d98e4b00679b4a830b2"},"self":{"href":"http://user/customers/57a98d98e4b00679b4a830b2"}}},{"firstName":"User1","lastName":"Name1","username":"user1","id":"57a98d98e4b00679b4a830b5","_links":{"addresses":{"href":"http://user/customers/57a98d98e4b00679b4a830b5/addresses"},"cards":{"href":"http://user/customers/57a98d98e4b00679b4a830b5/cards"},"customer":{"href":"http://user/customers/57a98d98e4b00679b4a830b5"},"self":{"href":"http://user/customers/57a98d98e4b00679b4a830b5"}}},{"firstName":"CATALINA","lastName":"pASCHINI","username":"CATA","id":"61369beeee11cb00019af5ea","_links":{"addresses":{"href":"http://user/customers/61369beeee11cb00019af5ea/addresses"},"cards":{"href":"http://user/customers/61369beeee11cb00019af5ea/cards"},"customer":{"href":"http://user/customers/61369beeee11cb00019af5ea"},"self":{"href":"http://user/customers/61369beeee11cb00019af5ea"}}}]}}

6. Lo esta ejecutando el user

7.cata@cata-UX550VD:~/Documents/Segundo Semestre/Ing_Software_III/socks-demo$ curl http://localhost/catalogue
[{"id":"03fef6ac-1896-4ce8-bd69-b798f85c6e0b","name":"Holy","description":"Socks fit for a Messiah. You too can experience walking in water with these special edition beauties. Each hole is lovingly proggled to leave smooth edges. The only sock approved by a higher power.","imageUrl":["/catalogue/images/holy_1.jpeg","/catalogue/images/holy_2.jpeg"],"price":99.99,"count":1,"tag":["action","magic"]},{"id":"3395a43e-2d88-40de-b95f-e00e1502085b","name":"Colourful","description":"proident occaecat irure et excepteur labore minim nisi amet irure","imageUrl":["/catalogue/images/colourful_socks.jpg","/catalogue/images/colourful_socks.jpg"],"price":18,"count":438,"tag":["brown","blue"]},{"id":"510a0d7e-8e83-4193-b483-e27e09ddc34d","name":"SuperSport XL","description":"Ready for action. Engineers: be ready to smash that next bug! Be ready, with these super-action-sport-masterpieces. This particular engineer was chased away from the office with a stick.","imageUrl":["/catalogue/images/puma_1.jpeg","/catalogue/images/puma_2.jpeg"],"price":15,"count":820,"tag":["sport","formal","black"]},{"id":"808a2de1-1aaa-4c25-a9b9-6612e8f29a38","name":"Crossed","description":"A mature sock, crossed, with an air of nonchalance.","imageUrl":["/catalogue/images/cross_1.jpeg","/catalogue/images/cross_2.jpeg"],"price":17.32,"count":738,"tag":["blue","action","red","formal"]},{"id":"819e1fbf-8b7e-4f6d-811f-693534916a8b","name":"Figueroa","description":"enim officia aliqua excepteur esse deserunt quis aliquip nostrud anim","imageUrl":["/catalogue/images/WAT.jpg","/catalogue/images/WAT2.jpg"],"price":14,"count":808,"tag":["green","formal","blue"]},{"id":"837ab141-399e-4c1f-9abc-bace40296bac","name":"Cat socks","description":"consequat amet cupidatat minim laborum tempor elit ex consequat in","imageUrl":["/catalogue/images/catsocks.jpg","/catalogue/images/catsocks2.jpg"],"price":15,"count":175,"tag":["brown","formal","green"]},{"id":"a0a4f044-b040-410d-8ead-4de0446aec7e","name":"Nerd leg","description":"For all those leg lovers out there. A perfect example of a swivel chair trained calf. Meticulously trained on a diet of sitting and Pina Coladas. Phwarr...","imageUrl":["/catalogue/images/bit_of_leg_1.jpeg","/catalogue/images/bit_of_leg_2.jpeg"],"price":7.99,"count":115,"tag":["blue","skin"]},{"id":"d3588630-ad8e-49df-bbd7-3167f7efb246","name":"YouTube.sock","description":"We were not paid to sell this sock. It's just a bit geeky.","imageUrl":["/catalogue/images/youtube_1.jpeg","/catalogue/images/youtube_2.jpeg"],"price":10.99,"count":801,"tag":["formal","geek"]},{"id":"zzz4f044-b040-410d-8ead-4de0446aec7e","name":"Classic","description":"Keep it simple.","imageUrl":["/catalogue/images/classic.jpg","/catalogue/images/classic2.jpg"],"price":12,"count":127,"tag":["brown","green"]}]

cata@cata-UX550VD:~/Documents/Segundo Semestre/Ing_Software_III/socks-demo$ curl http://localhost/tags
{"tags":["brown","geek","formal","blue","skin","red","action","sport","black","magic","green"],"err":null}

