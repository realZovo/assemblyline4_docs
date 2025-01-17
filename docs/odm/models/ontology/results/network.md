[comment]: # (AUTOGENERATED MARKDOWN CONTENT. UPDATES TO ODM DOCUMENTATION SHOULD BE DONE THROUGH ASSEMBLYLINE-BASE REPO!)
# NetworkConnection
> Details for a low-level network connection by IP

| Field | Type | Description | Required | Default |
| :--- | :--- | :--- | :--- | :--- |
| objectid | [ObjectID](/assemblyline4_docs/odm/models/ontology/results/process/#objectid) | The object ID of the network object | :material-checkbox-marked-outline: Yes | `None` |
| process | [Process](/assemblyline4_docs/odm/models/ontology/results/process/#process) | The process that spawned the network connection | :material-minus-box-outline: Optional | `None` |
| source_ip | IP | The source IP of the connection | :material-minus-box-outline: Optional | `None` |
| source_port | Integer | The source port of the connection | :material-minus-box-outline: Optional | `None` |
| destination_ip | IP | The destination IP of the connection | :material-checkbox-marked-outline: Yes | `None` |
| destination_port | Integer | The destination port of the connection | :material-checkbox-marked-outline: Yes | `None` |
| transport_layer_protocol | Enum | The transport layer protocol of the connection<br>Values:<br>`"tcp", "udp"` | :material-checkbox-marked-outline: Yes | `None` |
| direction | Enum | The direction of the network connection<br>Values:<br>`"inbound", "outbound", "unknown"` | :material-checkbox-marked-outline: Yes | `None` |
| http_details | [NetworkHTTP](/assemblyline4_docs/odm/models/ontology/results/network/#networkhttp) | HTTP-specific details of request | :material-minus-box-outline: Optional | `None` |
| dns_details | [NetworkDNS](/assemblyline4_docs/odm/models/ontology/results/network/#networkdns) | DNS-specific details of request | :material-minus-box-outline: Optional | `None` |
| connection_type | Enum | None<br>Values:<br>`"dns", "http"` | :material-minus-box-outline: Optional | `None` |


[comment]: # (AUTOGENERATED MARKDOWN CONTENT. UPDATES TO ODM DOCUMENTATION SHOULD BE DONE THROUGH ASSEMBLYLINE-BASE REPO!)
## NetworkDNS
> Details for a DNS request

| Field | Type | Description | Required | Default |
| :--- | :--- | :--- | :--- | :--- |
| domain | Domain | The domain requested | :material-checkbox-marked-outline: Yes | `None` |
| resolved_ips | List [IP] | A list of IPs that were resolved | :material-checkbox-marked-outline: Yes | `None` |
| lookup_type | Enum | The type of DNS request<br>Values:<br>`"A", "AAAA", "AFSDB", "ALIAS", "APL", "CAA", "CDNSKEY", "CDS", "CERT", "CNAME", "CSYNC", "DHCID", "DLV", "DNAME", "DNSKEY", "DS", "EUI48", "EUI64", "HINFO", "HIP", "HTTPS", "IPSECKEY", "KEY", "KX", "LOC", "MX", "NAPTR", "NS", "NSEC", "NSEC3", "NSEC3PARAM", "OPENPGPKEY", "PTR", "RP", "RRSIG", "SIG", "SMIMEA", "SOA", "SRV", "SSHFP", "SVCB", "TA", "TKEY", "TLSA", "TSIG", "TXT", "URI", "ZONEMD"` | :material-checkbox-marked-outline: Yes | `None` |


[comment]: # (AUTOGENERATED MARKDOWN CONTENT. UPDATES TO ODM DOCUMENTATION SHOULD BE DONE THROUGH ASSEMBLYLINE-BASE REPO!)
## NetworkHTTP
> Details for an HTTP request

| Field | Type | Description | Required | Default |
| :--- | :--- | :--- | :--- | :--- |
| request_uri | URI | The URI requested | :material-checkbox-marked-outline: Yes | `None` |
| request_headers | Mapping [Json] | Headers included in the request | :material-checkbox-marked-outline: Yes | `None` |
| request_body | Text | The body of the request | :material-minus-box-outline: Optional | `None` |
| request_method | Enum | The method of the request<br>Values:<br>`"BCOPY", "BDELETE", "BMOVE", "BPROPFIND", "BPROPPATCH", "CONNECT", "COPY", "DELETE", "GET", "HEAD", "LOCK", "MKCOL", "MOVE", "NOTIFY", "OPTIONS", "PATCH", "POLL", "POST", "PROPFIND", "PROPPATCH", "PUT", "SEARCH", "SUBSCRIBE", "TRACE", "UNLOCK", "UNSUBSCRIBE", "X-MS-ENUMATTS"` | :material-checkbox-marked-outline: Yes | `None` |
| response_headers | Mapping [Json] | Headers included in the response | :material-checkbox-marked-outline: Yes | `None` |
| response_status_code | Integer | The status code of the response | :material-minus-box-outline: Optional | `None` |
| response_body | Text | The body of the response | :material-minus-box-outline: Optional | `None` |


