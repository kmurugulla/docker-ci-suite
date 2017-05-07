FROM openjdk:8-jre
MAINTAINER Kiran Murugulla <Kiran.Murugulla@hotmail.com>

RUN apt-get update && apt-get install -qy apt-transport-https
RUN echo "deb http://downloads.sourceforge.net/project/sonar-pkg/deb binary/" >> /etc/apt/sources.list
RUN apt-get update && apt-get clean ### Sonar version 5.6 - timestamp

RUN apt-get install -y --force-yes sonar=5.6

COPY assets/init /app/init
RUN chmod 755 /app/init

VOLUME /opt/sonar/extensions
VOLUME /opt/sonar/logs/

ENTRYPOINT ["/app/init"]
CMD ["app:start"]