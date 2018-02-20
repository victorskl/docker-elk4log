# Look https://www.docker.elastic.co/ for specific version
FROM docker.elastic.co/kibana/kibana-oss:6.2.1

USER root

# Set the timezone - CentOS
ENV TZ=Australia/Melbourne
#RUN timedatectl set-timezone $TZ
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime

USER kibana

# Add your kibana plugins setup here
# Example: RUN kibana-plugin install <name|url>

#RUN kibana-plugin remove x-pack