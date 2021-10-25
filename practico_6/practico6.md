cata@cata-UX550VD:~/Documents/Segundo Semestre/Ing_Software_III/ing-software-3/practico_6/proyectos/spring-boot$ mvn clean package spring-boot:repackage
[INFO] Scanning for projects...
[INFO] 
[INFO] --------< org.springframework.boot:spring-boot-sample-actuator >--------
[INFO] Building Spring Boot Actuator Sample 2.0.2
[INFO] --------------------------------[ jar ]---------------------------------
[INFO] 
[INFO] --- maven-clean-plugin:3.0.0:clean (default-clean) @ spring-boot-sample-actuator ---
[INFO] Deleting /home/cata/Documents/Segundo Semestre/Ing_Software_III/ing-software-3/practico_6/proyectos/spring-boot/target
[INFO] 
[INFO] --- maven-resources-plugin:3.0.1:resources (default-resources) @ spring-boot-sample-actuator ---
[INFO] Using 'UTF-8' encoding to copy filtered resources.
[INFO] Copying 1 resource
[INFO] Copying 1 resource
[INFO] 
[INFO] --- maven-compiler-plugin:3.7.0:compile (default-compile) @ spring-boot-sample-actuator ---
[INFO] Changes detected - recompiling the module!
[INFO] Compiling 6 source files to /home/cata/Documents/Segundo Semestre/Ing_Software_III/ing-software-3/practico_6/proyectos/spring-boot/target/classes
[INFO] 
[INFO] --- maven-resources-plugin:3.0.1:testResources (default-testResources) @ spring-boot-sample-actuator ---
[INFO] Using 'UTF-8' encoding to copy filtered resources.
[INFO] Copying 1 resource
[INFO] 
[INFO] --- maven-compiler-plugin:3.7.0:testCompile (default-testCompile) @ spring-boot-sample-actuator ---
[INFO] Changes detected - recompiling the module!
[INFO] Compiling 3 source files to /home/cata/Documents/Segundo Semestre/Ing_Software_III/ing-software-3/practico_6/proyectos/spring-boot/target/test-classes
[INFO] 
[INFO] --- maven-surefire-plugin:2.9:test (default-test) @ spring-boot-sample-actuator ---
[INFO] Surefire report directory: /home/cata/Documents/Segundo Semestre/Ing_Software_III/ing-software-3/practico_6/proyectos/spring-boot/target/surefire-reports

-------------------------------------------------------
 T E S T S
-------------------------------------------------------
Running sample.actuator.ExampleInfoContributorTest
Tests run: 1, Failures: 0, Errors: 0, Skipped: 0, Time elapsed: 0.361 sec
Running sample.actuator.HelloWorldServiceTest
Tests run: 1, Failures: 0, Errors: 0, Skipped: 0, Time elapsed: 0 sec

Results :

Tests run: 2, Failures: 0, Errors: 0, Skipped: 0

[INFO] 
[INFO] --- maven-jar-plugin:3.0.2:jar (default-jar) @ spring-boot-sample-actuator ---
[INFO] --- spring-boot-maven-plugin:2.0.2.RELEASE:repackage (default-cli) @ spring-boot-sample-actuator ---
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  54.400 s
[INFO] Finished at: 2021-10-17T17:00:47-03:00
[INFO] ------------------------------------------------------------------------

Luego de la creacion del Dockerfile se obtuvo la siguiente salida
cata@cata-UX550VD:~/Documents/Segundo Semestre/Ing_Software_III/ing-software-3/practico_6/proyectos/spring-boot$ sudo docker build -t test-spring-boot . 
[sudo] password for cata: 
Sending build context to Docker daemon  21.81MB
Step 1/7 : FROM java:8-jre-alpine
8-jre-alpine: Pulling from library/java
709515475419: Already exists 
38a1c0aaa6fd: Pull complete 
cd134db5e982: Pull complete 
Digest: sha256:6a8cbe4335d1a5711a52912b684e30d6dbfab681a6733440ff7241b05a5deefd
Status: Downloaded newer image for java:8-jre-alpine
 ---> fdc893b19a14
Step 2/7 : RUN apk add --no-cache bash
 ---> Running in db0f23f20c5d
fetch http://dl-cdn.alpinelinux.org/alpine/v3.4/main/x86_64/APKINDEX.tar.gz
fetch http://dl-cdn.alpinelinux.org/alpine/v3.4/community/x86_64/APKINDEX.tar.gz
(1/5) Installing ncurses-terminfo-base (6.0_p20171125-r0)
(2/5) Installing ncurses-terminfo (6.0_p20171125-r0)
(3/5) Installing ncurses-libs (6.0_p20171125-r0)
(4/5) Installing readline (6.3.008-r4)
(5/5) Installing bash (4.3.42-r5)
Executing bash-4.3.42-r5.post-install
Executing busybox-1.24.2-r13.trigger
OK: 113 MiB in 39 packages
Removing intermediate container db0f23f20c5d
 ---> 0901ec81e1c4
Step 3/7 : WORKDIR /app
 ---> Running in 151e90a13e46
Removing intermediate container 151e90a13e46
 ---> 2d14d1925c64
Step 4/7 : COPY target/*.jar ./spring-boot-application.jar
 ---> 7b2e4ba4173c
Step 5/7 : ENV JAVA_OPTS="-Xms32m -Xmx128m"
 ---> Running in a7fe99f735de
Removing intermediate container a7fe99f735de
 ---> 516ba39c9813
Step 6/7 : EXPOSE 8080
 ---> Running in 3c23d2ec08d1
Removing intermediate container 3c23d2ec08d1
 ---> 0d440720b0b2
Step 7/7 : ENTRYPOINT exec java $JAVA_OPTS -Djava.security.egd=file:/dev/./urandom -jar spring-boot-application.jar
 ---> Running in 2282378c9c74
Removing intermediate container 2282378c9c74
 ---> a3bb64a70c3a
Successfully built a3bb64a70c3a


cata@cata-UX550VD:~/Documents/Segundo Semestre/Ing_Software_III/ing-software-3/practico_6/proyectos/spring-boot$ sudo docker run -p 8080:8080 test-spring-boot
docker: Error response from daemon: driver failed programming external connectivity on endpoint elegant_dijkstra (98c744d9c2ff16b4fd5bfe85dd8207bfec16dd84a6a13902df3cf085f073f436): Bind for 0.0.0.0:8080 failed: port is already allocated.
ERRO[0000] error waiting for container: context canceled 
cata@cata-UX550VD:~/Documents/Segundo Semestre/Ing_Software_III/ing-software-3/practico_6/proyectos/spring-boot$ curl -v localhost:8080
*   Trying 127.0.0.1:8080...
* TCP_NODELAY set
* Connected to localhost (127.0.0.1) port 8080 (#0)
> GET / HTTP/1.1
> Host: localhost:8080
> User-Agent: curl/7.68.0
> Accept: */*
> 
* Mark bundle as not supporting multiuse
< HTTP/1.1 302 Found
< Location: /dashboard/
< Date: Sun, 17 Oct 2021 20:10:41 GMT
< Content-Length: 34
< Content-Type: text/html; charset=utf-8
< 
<a href="/dashboard/">Found</a>.

* Connection #0 to host localhost left intact


No se puede correr la aplicacion ya que esta corriendo previamente otra cosa antes 

cata@cata-UX550VD:~/Documents/Segundo Semestre/Ing_Software_III/ing-software-3/practico_6/proyectos/spring-boot$ sudo docker build -t test-spring-boot .
Sending build context to Docker daemon  21.82MB
Step 1/13 : FROM maven:3.5.2-jdk-8-alpine AS MAVEN_TOOL_CHAIN
 ---> 293423a981a7
Step 2/13 : COPY pom.xml /tmp/
 ---> Using cache
 ---> 8bcce71d187e
Step 3/13 : RUN mvn -B dependency:go-offline -f /tmp/pom.xml -s /usr/share/maven/ref/settings-docker.xml
 ---> Running in 194ebc327885
[INFO] Scanning for projects...
INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 37:19 min
[INFO] Finished at: 2021-10-17T21:10:53Z
[INFO] Final Memory: 28M/138M
[INFO] ------------------------------------------------------------------------
(El docker file lo explique en comentarios)

EJERCICIO 4 
ata@cata-UX550VD:~/Documents/Segundo Semestre/Ing_Software_III/ing-software-3/practico_6/proyectos/python-flask$ sudo docker-compose up -d
[sudo] password for cata: 
Creating network "python-flask_default" with the default driver
Creating volume "python-flask_redis_data" with default driver
Pulling redis (redis:3.2-alpine)...
3.2-alpine: Pulling from library/redis
4fe2ade4980c: Already exists
fb758dc2e038: Pull complete
989f7b0c858b: Pull complete
42b4b9f869ad: Pull complete
17e06138ef20: Pull complete
c0ecd66db81e: Pull complete
Digest: sha256:e9083e10f5f81d350a3f687d582aefd06e114890b03e7f08a447fa1a1f66d967
Status: Downloaded newer image for redis:3.2-alpine
Building app
Sending build context to Docker daemon  6.656kB
Step 1/9 : FROM python:3.6.3
3.6.3: Pulling from library/python
f49cf87b52c1: Already exists 
7b491c575b06: Already exists 
b313b08bab3b: Already exists 
51d6678c3f0e: Already exists 
09f35bd58db2: Already exists 
1bda3d37eead: Already exists 
9f47966d4de2: Already exists 
9fd775bfe531: Already exists 
Digest: sha256:cdef88d8625cf50ca705b7abfe99e8eb33b889652a9389b017eb46a6d2f1aaf3
Status: Downloaded newer image for python:3.6.3
 ---> a8f7167de312
Step 2/9 : ENV BIND_PORT 5000
 ---> Running in 53260586ae3d
Removing intermediate container 53260586ae3d
 ---> 7ba1bec9dfde
Step 3/9 : ENV REDIS_HOST localhost
 ---> Running in 046d03d3705c
Removing intermediate container 046d03d3705c
 ---> 8cc3700abd77
Step 4/9 : ENV REDIS_PORT 6379
 ---> Running in 3b178111770a
Removing intermediate container 3b178111770a
 ---> 0a724dd0b882
Step 5/9 : COPY ./requirements.txt /requirements.txt
 ---> 63dc017cbbc2
Step 6/9 : RUN pip install -r /requirements.txt
 ---> Running in 1a32fe45560c
Collecting flask==1.0.2 (from -r /requirements.txt (line 1))
  Downloading https://files.pythonhosted.org/packages/7f/e7/08578774ed4536d3242b14dacb4696386634607af824ea997202cd0edb4b/Flask-1.0.2-py2.py3-none-any.whl (91kB)
Collecting redis==2.10.6 (from -r /requirements.txt (line 2))
  Downloading https://files.pythonhosted.org/packages/3b/f6/7a76333cf0b9251ecf49efff635015171843d9b977e4ffcf59f9c4428052/redis-2.10.6-py2.py3-none-any.whl (64kB)
Collecting Jinja2>=2.10 (from flask==1.0.2->-r /requirements.txt (line 1))
  Downloading https://files.pythonhosted.org/packages/94/42/d8bca8e99789bcc35dfa9b03acaa8b518720d6e060163745bc2bf2ead842/Jinja2-3.0.2-py3-none-any.whl (133kB)
Collecting Werkzeug>=0.14 (from flask==1.0.2->-r /requirements.txt (line 1))
  Downloading https://files.pythonhosted.org/packages/1e/73/51137805d1b8d97367a8a77cae4a792af14bb7ce58fbd071af294c740cf0/Werkzeug-2.0.2-py3-none-any.whl (288kB)
Collecting itsdangerous>=0.24 (from flask==1.0.2->-r /requirements.txt (line 1))
  Downloading https://files.pythonhosted.org/packages/9c/96/26f935afba9cd6140216da5add223a0c465b99d0f112b68a4ca426441019/itsdangerous-2.0.1-py3-none-any.whl
Collecting click>=5.1 (from flask==1.0.2->-r /requirements.txt (line 1))
  Downloading https://files.pythonhosted.org/packages/48/58/c8aa6a8e62cc75f39fee1092c45d6b6ba684122697d7ce7d53f64f98a129/click-8.0.3-py3-none-any.whl (97kB)
Collecting MarkupSafe>=2.0 (from Jinja2>=2.10->flask==1.0.2->-r /requirements.txt (line 1))
  Downloading https://files.pythonhosted.org/packages/fc/d6/57f9a97e56447a1e340f8574836d3b636e2c14de304943836bd645fa9c7e/MarkupSafe-2.0.1-cp36-cp36m-manylinux1_x86_64.whl
Collecting dataclasses; python_version < "3.7" (from Werkzeug>=0.14->flask==1.0.2->-r /requirements.txt (line 1))
  Downloading https://files.pythonhosted.org/packages/fe/ca/75fac5856ab5cfa51bbbcefa250182e50441074fdc3f803f6e76451fab43/dataclasses-0.8-py3-none-any.whl
Collecting importlib-metadata; python_version < "3.8" (from click>=5.1->flask==1.0.2->-r /requirements.txt (line 1))
  Downloading https://files.pythonhosted.org/packages/71/c2/cb1855f0b2a0ae9ccc9b69f150a7aebd4a8d815bd951e74621c4154c52a8/importlib_metadata-4.8.1-py3-none-any.whl
Collecting typing-extensions>=3.6.4; python_version < "3.8" (from importlib-metadata; python_version < "3.8"->click>=5.1->flask==1.0.2->-r /requirements.txt (line 1))
  Downloading https://files.pythonhosted.org/packages/74/60/18783336cc7fcdd95dae91d73477830aa53f5d3181ae4fe20491d7fc3199/typing_extensions-3.10.0.2-py3-none-any.whl
Collecting zipp>=0.5 (from importlib-metadata; python_version < "3.8"->click>=5.1->flask==1.0.2->-r /requirements.txt (line 1))
  Downloading https://files.pythonhosted.org/packages/bd/df/d4a4974a3e3957fd1c1fa3082366d7fff6e428ddb55f074bf64876f8e8ad/zipp-3.6.0-py3-none-any.whl
Installing collected packages: MarkupSafe, Jinja2, dataclasses, Werkzeug, itsdangerous, typing-extensions, zipp, importlib-metadata, click, flask, redis
Successfully installed Jinja2-3.0.2 MarkupSafe-2.0.1 Werkzeug-2.0.2 click-8.0.3 dataclasses-0.8 flask-1.0.2 importlib-metadata-4.8.1 itsdangerous-2.0.1 redis-2.10.6 typing-extensions-3.10.0.2 zipp-3.6.0
You are using pip version 9.0.1, however version 21.3 is available.
You should consider upgrading via the 'pip install --upgrade pip' command.
Removing intermediate container 1a32fe45560c
 ---> 512af3cf39e0
Step 7/9 : COPY ./app.py /app.py
 ---> 1b6f7a83f412
Step 8/9 : EXPOSE $BIND_PORT
 ---> Running in 651218d92430
Removing intermediate container 651218d92430
 ---> 81c87a70045f
Step 9/9 : CMD [ "python", "/app.py" ]
 ---> Running in c7d60aa7849a
Removing intermediate container c7d60aa7849a
 ---> 6301317a0268
Successfully built 6301317a0268
Successfully tagged python-flask_app:latest
WARNING: Image for service app was built because it did not already exist. To rebuild this image you must use `docker-compose build` or `docker-compose up --build`.
Creating python-flask_redis_1 ... done
Creating python-flask_app_1   ... done

El comando docker-compose up utiliza el archivo docker-compose.yml para asi leerlo y levantar el contenedor con todos los servicios especificados en el mismo.
Segun el docker-compose.yml declara que la aplicación va a estar en el contexto de redis, corriendo en el puerto 5000 con la imagen de redis redis:3.2-alpine


EJERCICIO 5
Creacion de la imagen:
cata@cata-UX550VD:~/Documents/Segundo Semestre/Ing_Software_III/ing-software-3/practico_6/proyectos/nodejs-docker$ sudo docker build -t test-node .
[sudo] password for cata: 
Sending build context to Docker daemon  188.6MB
Step 1/9 : FROM node:13.12.0-alpine AS build
 ---> 483343d6c5f5
Step 2/9 : WORKDIR /app
 ---> Using cache
 ---> 5beabad5a36d
Step 3/9 : COPY ./ ./
 ---> Using cache
 ---> dfa89b03f67f
Step 4/9 : RUN npm install --only=production
 ---> Using cache
 ---> 20990f557afc
Step 5/9 : FROM node:13.12.0-alpine AS release
 ---> 483343d6c5f5
Step 6/9 : WORKDIR /app
 ---> Using cache
 ---> 5beabad5a36d
Step 7/9 : COPY --from=build /app ./
 ---> Using cache
 ---> 15f2a8fc86da
Step 8/9 : EXPOSE 3000
 ---> Using cache
 ---> b79f2e111e48
Step 9/9 : CMD ["npm", "start"]
 ---> Using cache
 ---> 72a069266fb9
Successfully built 72a069266fb9
Successfully tagged test-node:latest



