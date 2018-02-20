# Look https://www.docker.elastic.co/ for specific version
FROM docker.elastic.co/elasticsearch/elasticsearch-oss:6.2.1

# Set the timezone - CentOS
ENV TZ=Australia/Melbourne
#RUN timedatectl set-timezone $TZ
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime

# Add your elasticsearch plugins setup here
# Example: RUN elasticsearch-plugin install analysis-icu

#RUN elasticsearch-plugin remove --purge x-pack