check_puppetdb_nodes
====================

Nagios/Icinga plugin for checking the health of your Puppet nodes via PuppetDB

## Icinga2 template

If you are using Icinga2, here is a command definition along with an example service that is automatically applied to all Linux hosts:

```
object CheckCommand "puppetdb-node" {
  import "plugin-check-command"

  command = [ LocalPluginDir + "/check_puppetdb_nodes" ]

  arguments = {
                "--hostname" = "$puppetdb_host$"
                "--port" = "$puppetdb_port$"
                "--node" = "$node_name$"
                "--warning" = "$warn_lag$"
                "--critical" = "$crit_lag$"
                "--warnfails" = "$warn_fails$"
                "--critfails" = "$crit_fails$"
                }

  vars.node_name = "$host_name$"
  vars.warn_lag = 45
  vars.crit_lag = 120
  vars.warn_fails = 1
  vars.crit_fails = 1
}

apply Service "puppet agent" {
  import "generic-service"

  check_command = "puppetdb-node"

  assign where (host.address || host.address6) && host.vars.os == "Linux"
}
```
