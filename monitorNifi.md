# Monitoring Nifi With Prometheus And Grafana

## Topics to be covered

1. Nifi Monitoring Integration with Promethus And Grafana
2. Site To Site Reporting
3. Email Alerting at Nifi




## Inital Set Up

Install Nifi : https://www.apache.org/dyn/closer.lua?path=/nifi/1.23.2/nifi-1.23.2-bin.zip

 ./bin/nifi.sh start
http://localhost:8080/nifi/


Install  Promethus : https://prometheus.io/download/

http://localhost:9090/targets?search=

 ./prometheus 


Install Grafana :

https://grafana.com/grafana/download?platform=mac
https://grafana.com/docs/grafana/latest/setup-grafana/installation/mac/

brew install grafana
brew services list
brew services start grafana

http://localhost:3000/?orgId=1



## Integrating Nifi with Prometheus 

1. Add a Prometheus reporting task 
2. Metrics will be available on port configured
3. Add this in prometheus.yml
4. Restart and you will be able to see nifi metrics


## Integrating with Grafana

1. Add Prometheus data source
2. Add a dashboard template
3.  Explore

https://grafana.com/grafana/dashboards/?search=nifi

https://grafana.com/grafana/dashboards/12314-nifi-prometheusreportingtask-dashboard/



## References


https://nifi.apache.org/docs.html
https://pierrevillard.com/2017/05/15/monitoring-nifi-workflow-sla/
https://www.youtube.com/watch?v=QqFgWh3xhuQ&t=10205s
https://www.youtube.com/watch?v=QLdgG_C9TFo


