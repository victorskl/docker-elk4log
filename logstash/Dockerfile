# Look https://www.docker.elastic.co/ for specific version
FROM docker.elastic.co/logstash/logstash-oss:6.2.1

USER root

# Set the timezone - CentOS
ENV TZ=Australia/Melbourne
#RUN timedatectl set-timezone $TZ
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime

USER logstash

# Add your logstash plugins setup here
# Example: RUN logstash-plugin install logstash-filter-json

#RUN logstash-plugin remove x-pack