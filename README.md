## Install metricbeat 

(Assumption:
You are using Elastic Cloud ( http://cloud.elastic.co ), or you already have existing ELK stack deployed. Whichever you choose, https://elastic.co/ start will get you started.)

Make sure that you take note of the CLOUD ID and Elastic Password if you are using Elastic Cloud or Elastic Cloud Enterprise.

You deploy Metricbeat as a DaemonSet to ensure that it is running on each node of the cluster. These instances are used to retrieve most metrics from the host, such as system metrics, Docker stats, and metrics from all the services running on top of Kubernetes.

To install metricbeat on our cluster follow the below mentioned steps: -


curl -L -O https://raw.githubusercontent.com/elastic/beats/8.2/deploy/kubernetes/metricbeat-kubernetes.yaml




By default, Metricbeat sends events to an existing Elasticsearch deployment, if present. To specify a different destination, change the following parameters in the manifest file:


        env:
        - name: ELASTICSEARCH_HOST
          value: elasticsearch
        - name: ELASTICSEARCH_PORT
          value: "9200"
        - name: ELASTICSEARCH_USERNAME
          value: elastic
        - name: ELASTICSEARCH_PASSWORD
          value: changeme
        - name: ELASTIC_CLOUD_ID
          value:
        - name: ELASTIC_CLOUD_AUTH
          value:



The values of ELASTICSEARCH_HOST will be your hostname, ELASTICSEARCH_PORT is the port number on which your elasticsearch is exposed. ELASTICSEARCH_USERNAME is the user you are using to access elasticsearch, ELASTICSEARCH_PASSWORD is the password that is used to access the elasticsearch, ELASTIC_CLOUD_ID and ELASTIC_CLOUD_AUTH you will only get these values if you are using the elastic cloud based elasticsearch.




To scrape metrics from kong, first you need to enable the prometheus module in metricbeat by adding the following section in configmap name metricbeat-daemonset-config  ( in the same downloaded file metricbeat-kubernetes.yaml) .



                - module: prometheus
                  period: 10s
                  metricsets: ["collector"]
                  hosts: ["ingress-kong.kong:8100"]
                  metrics_path: /metrics
                  headers:
                    accept: "text/plain"


## Visualizing metrics in Kibana 

Now everything is deployed we can go to our Kibana to check if the metrics are received or not from metricbeat to ElasticSearch. Open your kibana console (If you are using elastic cloud open your elasticloud account and select your deployment name). 

Once you're in, click on the 3 bars on the top left and select metrics. One metric window will appear. Click on inventory on the left-hand side and click on inventory on the left-hand side, click on a metric for more options and then click add metric.

Type prometheus to list  all prometheus metrics.  From the list you can select what metrics you want to get displayed.

Select a metric and give it a label so that it will be easy to recognise later from drop down menu. 
Now click save.





It will appear like this in the drop down menu.
