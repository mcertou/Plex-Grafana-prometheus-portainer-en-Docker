# Raspberry_Pi_4
-----------------

Configuración de los "docker-compose" para generar los siguientes contenedores en Docker:

- PLEX
- GRAFANA, NODE EXPORTER y PROMETHEUS
- PORTAINER

1) Primer paso crear la carpeta: /home/pi/docker
   -----------------------------------------------
2) Tener en cuenta la carpetas que se deben tener creadas para atar a los volumentes.
   -----------------------------------------------------------------------------------
 - Por ejemplo en plex: (Hay que crear las carpatas "Videos" o "Music"

    volumes:
      - /home/pi/docker/plex/config:/config
      - /home/pi/Videos:/movies
      - /home/pi/Music:/music
 

3) Para generar estos contenedores solo es preciso acceder a cada carpeta de los sericios que se quiere genear y lanzar:
   ----------------------------------------------------------------------------------------------------------------------

docker-compose up -d

4) Verificaciones:
   --------------

Para vertificar que los servicios están creados

pi@raspberrypi:~/docker $ docker ps -a
CONTAINER ID   IMAGE                       COMMAND                  CREATED        STATUS        PORTS                                       NAMES
b9497c6fd691   grafana/grafana             "/run.sh"                17 hours ago   Up 11 hours   0.0.0.0:3000->3000/tcp, :::3000->3000/tcp   grafana
a5e17e58ce01   prom/prometheus:latest      "/bin/prometheus --c…"   17 hours ago   Up 11 hours   0.0.0.0:9090->9090/tcp, :::9090->9090/tcp   prometheus
8fea71a75c27   prom/node-exporter:latest   "/bin/node_exporter …"   17 hours ago   Up 11 hours   0.0.0.0:9100->9100/tcp, :::9100->9100/tcp   node-exporter
e220c58f0007   portainer/portainer         "/portainer -H unix:…"   18 hours ago   Up 11 hours   0.0.0.0:9500->9000/tcp, :::9500->9000/tcp   portainer_portainer_1
227b8208eaa0   jaymoulin/plex:1.16.5       "daemon-pms"             22 hours ago   Up 11 hours    

5) Acceso Web de los servicios creados
   -----------------------------------

pi@raspberrypi:~/docker/prometheus $ docker-compose ps
    Name                   Command               State                    Ports
-------------------------------------------------------------------------------------------------
grafana         /run.sh                          Up      0.0.0.0:3000->3000/tcp,:::3000->3000/tcp
node-exporter   /bin/node_exporter --path. ...   Up      0.0.0.0:9100->9100/tcp,:::9100->9100/tcp
prometheus      /bin/prometheus --config.f ...   Up      0.0.0.0:9090->9090/tcp,:::9090->9090/tcp 

6) Verificaciones de trazas de logs
   --------------------------------

pi@raspberrypi:~/docker/prometheus $ docker-compose logs node-exporter
Attaching to node-exporter
node-exporter    | level=info ts=2021-10-02T15:36:38.453Z caller=node_exporter.go:182 msg="Starting node_exporter" version="(version=1.2.2, branch=HEAD, revision=26645363b486e12be40af7ce4fc91e731a33104e)"
node-exporter    | level=info ts=2021-10-02T15:36:38.453Z caller=node_exporter.go:183 msg="Build context" build_context="(go=go1.16.7, user=root@1c3411405b8f, date=20210806-13:44:06)"

