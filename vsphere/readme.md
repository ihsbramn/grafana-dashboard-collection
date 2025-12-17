This integration uses the otelcol.receiver.vcenter component to collect VMware vSphere metrics from a vCenter Server. Configure the following properties according to your environment:

endpoint: This must be set to the vCenter Server. The expected format is <protocol>://<hostname>:<port>.
username: This must be set to the user used to collect metrics from the vCenter Server.
password: This must be set to the password for the user used to collected metrics from the vCenter Server.
tls: Here a user must set options based on the vCenter Server's TLS configuration.
These metrics are first fed into the otelcol.processor.batch component to reduce the number of outgoing network requests required to transmit data.

Then they are fed into the otelcol.processor.transform component which will add a job label with a value of integrations/vsphere onto every metric.

Finally, the otelcol.exporter.prometheus component is used to convert the OTLP formatted metrics to Prometheus formatted metrics. Here OTEL resource attributes are converted to prometheus labels on each metric as well.

To monitor your VMware Appliance Management Service (applmgmt), VMware Analytics (analytics), and VMware vCenter Server (vpxd) logs, you will use one of the two following components.

For systems where Alloy is intercepting incoming syslogs: The loki.source.syslog component defines a syslog listener and the list of receivers to forward syslogs to. Change the following properties according to your environment:

address: This <vcenter-host:vcenter-syslog-port> address will inform the syslog component about where to listen in order to receive syslogs from the vCenter Server's remote syslog forwarding configuration.
<vcenter-host> must match the vCenter Server's IP address
<vcenter-syslog-port> must match the vCenter Server's remote syslog forwarding configuration.
protocol: Can be set to either tcp or udp.
For systems where Alloy is reading from a syslog file: The loki.source.file component reads the log entries from the syslog files, then forwards them to the receiver. Change the follow properties according to your environment:

targets: The list of syslog files to read from.
Combine one of the former components with the loki.process component to define how to process logs before sending them to Loki.