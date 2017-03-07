# ha-daemon
## Simple HA service daemon


# Contributors are more than welcome !


### HA-DAEMON represents complete yet simple HA solution. It consists of two components:

    Port trigger that is following main resource - this can be excluded in case resurce is having it's own pingable tcp port
    Main HA process that is setting main resource state based on condition

### Main file locations are:

    /usr/local/sbin/haping - port trigger
    /usr/local/sbin/ha-daemon.py - main HA module
    /etc/ha.cfg - main config file
    /var/log/ha-daemon.log - logfile
    /etc/init.d/ha-daemon

 

haping is appended in the resource  startup so that it can reserve HA port when the service is running.

### HA is working in master-slave mode. Roles are defined in the ha.cfg file:
```
  [main]
  isMaster = True #is node master or not(True|False)
  interface = ens192 #observed interface name
  masterHost = 10.10.3.3 #master node ip
  slaveHost = 10.10.3.9 #slave node ip
  gw = 10.10.3.1 #gateway IP that needs(or not) to be reached by node from $interface
  port = 55000 #port where the socket digger is listening
  procname = nginx #resource process name
  interval = 10 #heartbeat interval
  resource = /etc/init.d/nginx #resource that is started | stopped
```
  
slave configuration has False for isMaster

Use /etc/init.d/ha-daemon is executed instead of /etc/init.d/nginx  as it will start also the nginx server !

 

In case Master node has network issue, script will terminate resourced service on in and slave will take over. Once master is back, slave will shutdown it's service and master will take over.

Script can be modified for any other resource by changing resource and procname.
