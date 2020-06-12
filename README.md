# zabbix-template-oxidized
A quick template to monitor Oxidized in Zabbix, including required Oxidized hooks. No value maps, I've left that up to your imagination. It will capture relevant hooks for polling devices, as well as push a 0 or 1 status for the last poll, and the time of the poll. Make sure you change the value of $ZBX_SERVER to your Zabbix Server or Proxy.

## Prerequisites
* zabbix
* zabbix_sender
* oxidized
* date
* A relatively recent shell capable of in-line exports

## Oxidized Hooks
Add these hooks to your hook: section (remember that it's YAML, so watch your spaces and indentation).
I've pre-indented this code block for your convenience, but you're responsible for your own YAML...
```
  node_success:
    type: exec
    events: [node_success]
    cmd: 'EPOCHTIME=`date +%s`; $ZBX_SERVER=CHANGEME;
    zabbix_sender -z $ZBX_SERVER -s $OX_NODE_IP -k OX_EVENT       -o $OX_EVENT;
    zabbix_sender -z $ZBX_SERVER -s $OX_NODE_IP -k OX_JOB_STATUS  -o $OX_JOB_STATUS;
    zabbix_sender -z $ZBX_SERVER -s $OX_NODE_IP -k OX_JOB_TIME    -o $OX_JOB_TIME;
    zabbix_sender -z $ZBX_SERVER -s $OX_NODE_IP -k OX_NODE_IP     -o $OX_NODE_IP;
    zabbix_sender -z $ZBX_SERVER -s $OX_NODE_IP -k OX_NODE_MODEL  -o $OX_NODE_MODEL;
    zabbix_sender -z $ZBX_SERVER -s $OX_NODE_IP -k OX_NODE_NAME   -o $OX_NODE_NAME;
    zabbix_sender -z $ZBX_SERVER -s $OX_NODE_IP -k oxidized.datetime -o $EPOCHTIME;
    zabbix_sender -z $ZBX_SERVER -s $OX_NODE_IP -k oxidized.status -o 1'
  node_failed:
    type: exec
    events: [node_fail]
    cmd: 'EPOCHTIME=`date +%s`; $ZBX_SERVER=CHANGEME;
    zabbix_sender -z $ZBX_SERVER -s $OX_NODE_IP -k OX_EVENT       -o $OX_EVENT;
    zabbix_sender -z $ZBX_SERVER -s $OX_NODE_IP -k OX_JOB_STATUS  -o $OX_JOB_STATUS;
    zabbix_sender -z $ZBX_SERVER -s $OX_NODE_IP -k OX_JOB_TIME    -o $OX_JOB_TIME;
    zabbix_sender -z $ZBX_SERVER -s $OX_NODE_IP -k OX_NODE_IP     -o $OX_NODE_IP;
    zabbix_sender -z $ZBX_SERVER -s $OX_NODE_IP -k OX_NODE_MODEL  -o $OX_NODE_MODEL;
    zabbix_sender -z $ZBX_SERVER -s $OX_NODE_IP -k OX_NODE_NAME   -o $OX_NODE_NAME;
    zabbix_sender -z $ZBX_SERVER -s $OX_NODE_IP -k oxidized.datetime -o $EPOCHTIME;
    zabbix_sender -z $ZBX_SERVER -s $OX_NODE_IP -k oxidized.status -o 0'
