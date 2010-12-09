# Nagios-MongoDB (errmsg)

## Overview

This is a simple Nagios check script to monitor your MongoDB servers errmsg specially if your setup is a replication setup (replset) . 
It checks the output of rs.status command and reports the status of each errmsg field corresponding to the node to which it belongs...


## Installation

In your Nagios plugins directory run

<pre><code>git clone git://github.com/traveliq/nagios_plugin_mongodb_errmsg.git</code></pre>

## Usage

### Install in Nagios

Edit your commands.cfg and add the following

<pre><code>
define command {
           command_name    check_mongo_errmsg
           command_line    /usr/local/nagios/libexec/check_mongo_errmsg $HOSTADDRESS$ $ARG1$ $ARG2$ 
}
</code></pre>

Where ARG1 is for warning and ARG2 for critical. 
Both are optional values - defaults are:

WARNING = 1
CRITICAL = 2 

Then you can reference it like the following. This is is my services.cfg

#### Check Connection

This will check each host that is listed in the Mongo Servers group. It will issue a warning if one of the nodes reports a error message and a critical error there are 2 or more reported errors.

<pre><code>
define service {
              use                     generic-service
              host_name               Mongo Server
              service_description     MongoDB errmsg
              check_command           check_mongo_errmsg!1!2
}
</code></pre>
