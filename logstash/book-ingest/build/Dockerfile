FROM docker.elastic.co/logstash/logstash:7.8.0

# Elastic containers have strict permissions, this ingestion technique is meant to be
# ephemeral and as such will simply be destroyed after completion of its task. 
# In other words, we can use root for this.
USER root

# Remove the default pipeline configuration in the container
RUN rm -f /usr/share/logstash/pipeline/logstash.conf

# Add our own configuration
ADD pipeline/ /usr/share/logstash/pipeline/

# Create a directory to mount our CSV/log/input file
RUN mkdir /data

# Create a volume for the /data directory
VOLUME ["/data"] 