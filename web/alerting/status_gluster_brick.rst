Status of Gluster Brick
***********************

:author: mbukatov@redhat.com

Description
===========

Alerts covered in this test case:

* brick status (volume, host, cluster)

Based on:

* `List of Alerts and Notifications in Tendrl`_
* :rhbz:`1517270`

Setup
=====

#. You need a gluster volume.

Test Steps
==========

.. test_action::
   :step:
       Turn off one of machine in Gluster storage pool.

   :result:
       Tendrl sends the following alerts:

       * "Status of brick changed from Started to Stopped".
         There is info about cluster ID and peer hostname in this alert.

.. test_action::
   :step:
       Turn on back one of machine in Gluster storage pool.

   :result:
       Tendrl sends the following alerts:
       
       * "Status changed of brick changed from Stopped to Started".
         There is info about cluster ID and peer hostname in alert.

.. test_action::
   :step:
       Turn off 5 from 6 machines in Gluster storage pool.

   :result:
       Tendrl sends the following alerts for arbiter_2_plus_1x2 volume:

       * "Status of brick changed from Started to Stopped".
         There is info about cluster ID and peer hostname in this alert.
       * "Afr Quorum State - Afr quorum has failed for subvolume"
         There is also info about sub-volume ID and cluster ID in this alert.
       * "Afr Subvol State - Afr subvolume is down"
         There is also info about sub-volume and cluster ID.
       
.. test_action::
   :step:
       Turn on back all machines in Gluster storage pool.

   :result:
       Tendrl sends the following alerts for arbiter_2_plus_1x2 volume:
       
       * "Status changed of brick changed from Stopped to Started".
         There is info about cluster ID and peer hostname in alert.
       * "Afr Quorum State - Afr quorum is met for subvolume"
         There is also info about sub-volume ID and cluster ID in this alert.
       * "Afr Subvol State - Afr subvolume is back up in cluster"
         There is also info about sub-volume and cluster ID.


Teardown
========

#. Make sure all machines and volumes used during testing are up again.
