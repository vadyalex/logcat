# logcat

### Description

Apache Tomcat 8 with JRE 8 based on official image that uses SLF4J + Logback + logstash-logback-encoder for logging.

### Usage

Run the default Tomcat server:

	docker run -it --rm -p 8080:8080 vadyalex/logcat

Mount Tomcat logs directory:

    docker run -it --rm -p 8080:8080 -v /somewhere/logs:/usr/local/tomcat/logs vadyalex/logcat


Local directory`/somewhere/logs` will contain `tomcat.log` and `tomcat.logstash`.
`tomcat.log` contains logs in human readable form. `tomcat.logstash` contains structured logs in Logstash JSON format that may be directly read by Logstash process:

```json
input {

  file {
    type => "tomcat"
    path => "/somewhere/logs/tomcat.logstash"
    codec => "json"
  }

}

output {

  stdout {
    codec => rubydebug
  }

}
```

`tomcat.log` and `tomcat.logstash` rotated and archived into GZIP file daily. Archives are stored for period of three days.

You can modify default Logback configuration providing custom `logback.xml` that is located at `/somewhere/logback` directory, run logcat image as follows:

        docker run -it --rm -p 8080:8080 -v /somewhere/logback:/usr/local/tomcat/conf/logback -v /somewhere/logs:/usr/local/tomcat/logs vadyalex/logcat


