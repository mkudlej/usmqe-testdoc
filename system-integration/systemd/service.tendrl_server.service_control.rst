Tendrl server services control test
************************************

:author: - mbukatov@redhat.com
         - mkudlej@redhat.com
:caseposneg: positive

Description
===========

Check that Tendrl services on server can be properly managed via systemd, this
test checks basic use cases for:

* start
* stop
* restart
* reload

There are other actions (such as ``try-restart``, ...) which we don't check so
far.

Setup
=====

Follow TODO to install Tendrl. No particular
cluster configuration is required for this test case, so we use the default
one.

All test steps are performed on the master server, where the Tendrl services 
is running.

Test Steps
==========

.. test_action::
   :step:
       Check status of the service::

           # systemctl status tendrl-api
           # systemctl status tendrl-monitoring-integration
           # systemctl status tendrl-node-agent

   :result:
       Status of the services are shown. Based on the setup section, services
       are expected to be both *running* and *enabled*.

.. test_action::
   :step:
       Stop the services::

           # systemctl stop tendrl-api
           # systemctl stop tendrl-monitoring-integration
           # systemctl stop tendrl-node-agent
   :result:
       Services are stopped. Verify by::

           # systemctl status tendrl-api
           # systemctl is-active tendrl-api
           # systemctl status tendrl-monitoring-integration
           # systemctl is-active tendrl-monitoring-integration
           # systemctl status tendrl-node-agent
           # systemctl is-active tendrl-node-agent

.. test_action::
   :step:
       Start the services again::

           # systemctl start tendrl-api
           # systemctl start tendrl-monitoring-integration
           # systemctl start tendrl-node-agent
   :result:
       Services are running. Verify by::

           # systemctl status tendrl-api
           # systemctl is-active tendrl-api
           # systemctl status tendrl-monitoring-integration
           # systemctl is-active tendrl-monitoring-integration
           # systemctl status tendrl-node-agent
           # systemctl is-active tendrl-node-agent

.. test_action::
   :step:
       Restart services (from the running state)::

           # systemctl restart tendrl-api
           # systemctl restart tendrl-monitoring-integration
           # systemctl restart tendrl-node-agent
   :result:
       Services have been restarted and are running now. Verify by::

           # systemctl status tendrl-api
           # systemctl is-active tendrl-api
           # systemctl status tendrl-monitoring-integration
           # systemctl is-active tendrl-monitoring-integration
           # systemctl status tendrl-node-agent
           # systemctl is-active tendrl-node-agent

       Check that `Active since` date has been updated.

.. test_action::
   :step:
       Stop services (again)::

           # systemctl stop tendrl-api
           # systemctl stop tendrl-monitoring-integration
           # systemctl stop tendrl-node-agent
   :result:
       Services are stopped. Verify by::

           # systemctl status tendrl-api
           # systemctl is-active tendrl-api
           # systemctl status tendrl-monitoring-integration
           # systemctl is-active tendrl-monitoring-integration
           # systemctl status tendrl-node-agent
           # systemctl is-active tendrl-node-agent

.. test_action::
   :step:
       Restart services (from the stopped state)::

           # systemctl restart tendrl-api
           # systemctl restart tendrl-monitoring-integration
           # systemctl restart tendrl-node-agent
   :result:
       Services have been restarted and are running now. Verify by::

           # systemctl status tendrl-api
           # systemctl is-active tendrl-api
           # systemctl status tendrl-monitoring-integration
           # systemctl is-active tendrl-monitoring-integration
           # systemctl status tendrl-node-agent
           # systemctl is-active tendrl-node-agent

       Check that `Active since` date has been updated.

.. test_action::
   :step:
       Reload configuration of services::

           # systemctl reload tendrl-api
           # systemctl reload tendrl-monitoring-integration
           # systemctl reload tendrl-node-agent
   :result:
       Commands return zero return code and tendrl-api, tendrl-monitoring-integration and
       tendrl-node-agent configurations have been reloaded. Check that 
       configuration files have been accessed::

           # find /etc/tendrl/ -type f | egrep -v "/etc/tendrl/monitoring-integration/grafana|/etc/tendrl/monitoring-integration/carbon.conf|/etc/tendrl/monitoring-integration/graphite-web.conf" | xargs stat | egrep '^Access: 2|File:'

       All config files have a new (recent, silimar to each other) access
       timestamp.

Teardown
========

Teardown is not needed.
