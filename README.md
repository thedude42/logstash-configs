# logstash-configs
Collection of my logstash configs

I use these configs to consume the following event streams
- syslog
- Bro (now Zeke) logs
- NetFlow exported via softflowd

## Directories

### conf.d/

Logstash configs targeting specific software

#### bro

Logstash inputs are for default broctl.cfg settings `LogDir = /var/log/bro` and `SpoolDir = /var/spool/bro`. The grok patterns are complete for conn.log, dns.log and dhcp.log. The file notice.log is incomplete (gets occasional \_grokparsefailure errors).

**TODO** need to do grok patterns for stats.log, ssl.log and x509.log. http.log and smtp.log is less interesting these days

#### netflow

Logstash input type `udp` with `codec => netflow`.

### patterns/

Logstash patterns used in the other Logstash configs. Some of these may be redundant, as none of these have been refactored since expected funtionality of defined patterns had been achieved.

#### filterlog.conf

Patterns for PfSense filterlog events

#### syslogfixes

Miscilaneous patterns to help simplify the grok patterns in the main Logstash configs for handling syslog events of various sources

### jinja2/

Jinja templates that produce a Logstash config.

#### ansible_template.j2

Jinja template that can be used with an Ansible inventory file and log paths of the format 
```
/var/log/remotes/{{ hostvars[node].inventory_hostname_short }}/{{ hostvars[node].inventory_hostname_short }}.log
```
