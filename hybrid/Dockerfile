FROM openjdk:8-slim

# install curl
RUN apt-get update; apt-get install -y curl

# install into the /opt directory
WORKDIR /opt

# download and install the latest storage server
RUN curl -sL https://app-takipi-com.s3.amazonaws.com/deploy/takipi-storage/takipi-storage-latest.tar.gz | tar -xvzf -

# use a volume to store data
VOLUME ["/opt/takipi-storage/storage"]

# use mount to make settings.yaml available
VOLUME ["/opt/takipi-storage/private"]

# create a run script
RUN echo "#!/bin/bash" > run.sh \
 && echo "java -jar /opt/takipi-storage/lib/takipi-storage.jar server /opt/takipi-storage/private/settings.yaml &> /opt/takipi-storage/log/takipi-storage.log &" >> run.sh \
 && echo "/usr/bin/tail -f /opt/takipi-storage/log/takipi-storage.log" >> run.sh \
 && chmod +x run.sh

EXPOSE 8080 8081

# run the service, printing logs to stdout
CMD ["./run.sh"]