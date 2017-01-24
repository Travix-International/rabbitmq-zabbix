rabbitmq-zabbix
=======================

Template and checks to monitor rabbitmq queues and server via Zabbix.

## Source:
https://github.com/Travix-International/rabbitmq-zabbix
Modified from original: https://github.com/jasonmcintosh/rabbitmq-zabbix

## What:
Set of python scripts, zabbix template, and associated data to do autodiscovery

## How:
1. Install the `rabbitmq_zabbix` python script in your agent extensions
   directory
2. Configure zabbix agent UserParameters (see below)
3. Import the template to your zabbix server
4. Make sure zabbix_sender is available on the agent server
5. Restart the zabbix agent


## Configuration:
You may optionally create a `.rab.auth` file in the `scripts/rabbitmq` directory.
This file allows you to change default parameters. The format is
`VARIABLE=value`, one per line. The default values are as follows:

    USERNAME=guest
    PASSWORD=guest
    CONF=/etc/zabbix/zabbix_agent.conf

You can also add a filter in this file to restrict which queues are monitored.
This item is a JSON-encoded string. The format provides some flexibility for
its use. You can either provide a single object or a list of objects to filter.
The available keys are: status, node, name, consumers, vhost, durable,
exclusive_consumer_tag, auto_delete, memory, policy

For example, the following filter could find all the durable queues:
`FILTER='{"durable": true}'`

To only use the durable queues for a given vhost, the filter would be:
`FILTER='{"durable": true, "vhost": "mine"}'`

To supply a list of queue names, the filter would be:
`FILTER='[{"name": "mytestqueuename"}, {"name": "queue2"}]'`

## Low level discovery of queues, including global regular expressions:
`https://www.zabbix.com/documentation/3.0/manual/regular_expressions`
The low level discovery, which is what determines what queues to be monitored,
requires with the existing template that a filter be defined as a global regular
expression. You can modify the template to do it in other ways, e.g. with a host
level macro (not tested), override it per host, or any number of methods. But
without a filter, *no* queues will be discovered, *just* server level items will
show up, and your checks will fail.

At some point the filters may be improved to include regular expressions or
"ignore these queues"

## CHANGES
* Changed template to work with Zabbix 2.2
* Changed file structure
* Added more checks and metrics to be exposed
* Updated to use zabbix_sender to push data on request to an item request.  This is similar to how the FromDual MySQL Zabbix stuff works and the concept was pulled from their templates.
* Updated the filters to handle a list of objects


## Ideas for the future?
Add a local cache of the results (may be overkill for RabbitMQ).
Feel free to submit changes or ideas - mcintoshj@gmail.com

## Definite kudos to some of the other developers around the web.  In particular,
* Python Scripts: https://github.com/kmcminn/rabbit-nagios
* Base idea for the Rabbit template:  https://github.com/alfss/zabbix-rabbitmq
* Also need to thank Lewis Franklin https://github.com/brolewis for his contributions!
* Base of this fork: https://github.com/jasonmcintosh/rabbitmq-zabbix
