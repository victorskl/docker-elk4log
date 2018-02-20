# Logspout extension

Logspout collects all Docker logs using the Docker logs API, and forwards them to Logstash without any additional
configuration.

## Usage

In your Logstash pipeline configuration, enable the `udp` input and set the input codec to `json`:

```
input {
  udp {
    port  => 5000
    codec => json
  }
}
```

## Documentation

https://github.com/looplab/logspout-logstash
