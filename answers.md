Your answers to the questions go here.
Lloyd Soldatt
DataDog, Sales Engineering, London

LSoldatt@Gmail.com 
+44 7799 213030

 Prerequisites - Set Up the Environment

-	DataDog Agent was installed on MacOS 13
-	
 
-	Environment was configured and is up and running
-	Output of current state with DD 'info' command

SLDTTMacBookAir:~ prism$ /usr/local/bin/datadog-agent info
====================
Collector (v 5.11.3)
====================

  Status date: 2018-01-29 10:05:11 (5s ago)
  Pid: 25405
  Platform: Darwin-17.4.0-x86_64-i386-64bit
  Python Version: 2.7.12, 64bit
  Logs: <stderr>, /var/log/datadog/collector.log, syslog:/var/run/syslog

  Clocks
  ======
  
    NTP offset: Unknown (No response received from 1.datadog.pool.ntp.org.)
    System UTC time: 2018-01-29 10:05:17.768102
  
  Paths
  =====
  
    conf.d: /opt/datadog-agent/etc/conf.d
    checks.d: /opt/datadog-agent/agent/checks.d
  
  Hostnames
  =========
  
    No host information available yet.
  
  Checks
  ======
  
    No checks have run yet.
  
  Emitters
  ========
  
    No emitters have run yet.

====================
Dogstatsd (v 5.11.3)
====================

  Status date: 2018-01-29 10:05:11 (6s ago)
  Pid: 25403
  Platform: Darwin-17.4.0-x86_64-i386-64bit
  Python Version: 2.7.12, 64bit
  Logs: <stderr>, /var/log/datadog/dogstatsd.log, syslog:/var/run/syslog

  Flush count: 0
  Packet Count: 0
  Packets per second: 0
  Metric count: 0
  Event count: 0
  Service check count: 0

====================
Forwarder (v 5.11.3)
====================

  Status date: 2018-01-29 10:05:16 (2s ago)
  Pid: 25404
  Platform: Darwin-17.4.0-x86_64-i386-64bit
  Python Version: 2.7.12, 64bit
  Logs: <stderr>, /var/log/datadog/forwarder.log, syslog:/var/run/syslog

  Queue Size: 0 bytes
  Queue Length: 0
  Flush Count: 1
  Transactions received: 0
  Transactions flushed: 0
  Transactions rejected: 0
  API Key Status: API Key is valid
  

SLDTTMacBookAir:~ prism$ /usr/local/bin/datadog-agent info
====================
Collector (v 5.11.3)
====================

  Status date: 2018-01-29 10:05:47 (19s ago)
  Pid: 25405
  Platform: Darwin-17.4.0-x86_64-i386-64bit
  Python Version: 2.7.12, 64bit
  Logs: <stderr>, /var/log/datadog/collector.log, syslog:/var/run/syslog

  Clocks
  ======
  
    NTP offset: 0.0021 s
    System UTC time: 2018-01-29 10:06:06.963100
  
  Paths
  =====
  
    conf.d: /opt/datadog-agent/etc/conf.d
    checks.d: /opt/datadog-agent/agent/checks.d
  
  Hostnames
  =========
  
    socket-hostname: SLDTTMacBookAir.home
    hostname: SLDTTMacBookAir.home
    socket-fqdn: sldttmacbookair.home
  
  Checks
  ======
  
    ntp
    ---
      - Collected 0 metrics, 0 events & 0 service checks
  
    disk
    ----
      - instance #0 [OK]
      - Collected 32 metrics, 0 events & 0 service checks
  
    mongo
    -----
      - instance #0 [OK]
      - Collected 334 metrics, 0 events & 1 service check
      - Dependencies:
          - pymongo: 3.2
  
    network
    -------
      - instance #0 [OK]
      - Collected 24 metrics, 0 events & 0 service checks
  
  
  Emitters
  ========
  
    - http_emitter [OK]

====================
Dogstatsd (v 5.11.3)
====================

  Status date: 2018-01-29 10:06:01 (5s ago)
  Pid: 25403
  Platform: Darwin-17.4.0-x86_64-i386-64bit
  Python Version: 2.7.12, 64bit
  Logs: <stderr>, /var/log/datadog/dogstatsd.log, syslog:/var/run/syslog

  Flush count: 5
  Packet Count: 0
  Packets per second: 0.0
  Metric count: 1
  Event count: 0
  Service check count: 0

====================
Forwarder (v 5.11.3)
====================

  Status date: 2018-01-29 10:06:06 (1s ago)
  Pid: 25404
  Platform: Darwin-17.4.0-x86_64-i386-64bit
  Python Version: 2.7.12, 64bit
  Logs: <stderr>, /var/log/datadog/forwarder.log, syslog:/var/run/syslog

  Queue Size: 0 bytes
  Queue Length: 0
  Flush Count: 17
  Transactions received: 6
  Transactions flushed: 6
  Transactions rejected: 0
  API Key Status: API Key is valid

-	DD Agent 'checks.d' and 'conf.d' folders, essential for the configuration of Integrations and bespoke Checks
  



 

Collecting Metrics

-	MongoDB 'admin' DB instance v3.6.2 installed on the system
-	MongoDB 'Admin' User configured within the 'admin' instance
-	DD User configured within this Mongo DB instance
-	My DD online portal - connected to LSoldatt@Gmail.com 
 



 

-	Host / MongoDB / NTP map of My Infrastructure 
 



-	Mongo DB V3.62 Integration


 


-	Collecting all standard and optional MongoDB metrics with 1 valid instance ('admin') connected to DD Dashboard
-	
 

 

-	Post within my DD online dashboard
 


-	Created the custom check 'my_metric' both in Python and in the corresponding YAML

-	YAML:

mymetric.yaml 

(tabs are not used in this YAML, just spaces for indentation for parsing)


init_config:

    min_collection_interval: 15

instances:

    [{}]

-	PY:

mymetric.py

from checks import AgentCheck
from random import * 
class MyMetricCheck(AgentCheck):
    def check(self, instance):
        randomInt = randint(1, 1001)
        self.gauge('my_metric',randomInt)


Bonus Question:

-	Yes, externalising the global parameter 'min collection interval', which defaults to 0, into the corresponding YAML file, will allow to update the YAML file without touching the PY class definition Agent Check itself. Thus, the interval can be set and updated in the corresponding YAML, as it must be.  In fact, this is the proper way of creating a controllable class in PY/Java, all key parameters need to be read in from an external YAML/interface/config files. 


Note: My PY and YAML for this Agent Check result in no errors, but explicit execution does not work as DD-Agent user is not the super user on my system (please, see the source code above to verify):

SLDTTMacBookAir:~ prism$ sudo -u dd-agent check MyMetricCheck
sudo: unknown user: dd-agent
sudo: unable to initialize policy plugin


Visualizing Data


-	Allocated both API and APP keys for my Timeboard script

 


-	Created the API script to create the Timeboard via DD API


from datadog import initialize, api

options = {
    'api_key': 'c754da805dfcf11ab28b07fa3064ebe8',
    'app_key': 'e97426651d8885ac22d09fb9a9b6cdc03026b1f4'
}

initialize(**options)

title = "Lloyd's Timeboard"
description = "Timeboard for MongoDB / MacOS."
graphs = [{
    "definition": {
        "events": [],
        "requests": [
            {"q": "my_metric{*}"},
            {"q": "mongodb.connections.current{*}"}
        ],
        "viz": "timeseries"
    },
    "title": "My Custom Metric & MongoDB Connections"
}]

template_variables = [{
    "name": "SLDTTMacBookAir.home",
    "prefix": "host",
    "default": "host:my-host"
}]

read_only = True
api.Timeboard.create(title=title,
                     description=description,
                     graphs=graphs,
                     template_variables=template_variables,
                     read_only=read_only)

Bonus Question: Anomaly function by DD is a very useful capability, which makes the solution extremely powerful in scenarios where hosted apps can produce sudden spikes of data/fluctuations, but which is perfectly normal for these applications, ex. MongoDB within an enterprise where query throughput grows exponentially at a certain hour of the day. A MongoDB instance can log a sudden spike of queries during the day and when we apply DD anomaly function, it will look at this app performance from the historical perspective. Thus, although, this spike may look as a problem in the context of now and today, it is predictable and was historically observed time and time before. DD anomaly function, when applied to a metric, will look at its logging performance in the context of its behaviour historically and will analyse whether this spike is, in fact, within this metric historically documented performance 'range'. Practically, it is also useful to apply this function over metrics known to fluctuate to avoid false alerts when, indeed, their behaviour is statistically within limits.

_end


