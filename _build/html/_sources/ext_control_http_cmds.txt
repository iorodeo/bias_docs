*********************************************
Extrenal Control via HTTP Commands
*********************************************

BIAS can be controlled via external HTTP commands when the external control
server is enabled. The can be done by checking the "Enabled" item under the
"Server" menu as shown below.


.. figure:: _static/bias_server_menu.png
   :align:  center


The port number used by the external control server, when enabled, is shown in
the second menu item - e.g. Port(5010).

Basic Command Structure
=======================

Commands are sent using http GET request as follows:

.. code-block:: bash 

   http://localhost:5010/?<commandName>=<commandArgs>
    
where "commandName" is the name of the and "commandArg" is the command argument
(possibley empty).  In this example the "localhost" is used for the devie
address and the port is assumed to be 5010. The local computers IP address may
used in place of "localhost". 

As an example consider the "stop-capture" command which instructs BIAS to stop
capturing images from the camera. This command can be issued as follows: 

.. code-block:: bash

    http://localhost:5010/?connect

In response to each command BIAS will return a JSON object containing the name
of the command, a flag indicating the success or failure of the command, a
message indicating the reason for failure (if failure occurred), and the
desired response value (if successful).  For example, the response to a
successful "set-video-file" command would be as follows: 

.. code-block:: javascript

    { 
        "command" : "set-video-file", 
        "message" : "", 
        "success" : true, 
        "value" : "" 
    }

List of Commands
================

This list of commands accepted by BIAS is as follows:
:ref:`section_win_install_gcc`

* :ref:`section_cmd_connect`
* :ref:`section_cmd_disconnect`
* :ref:`section_cmd_start-capture`
* :ref:`section_cmd_stop-capture`
* :ref:`section_cmd_get-configuration`
* :ref:`section_cmd_set-configuration`
* :ref:`section_cmd_enable-logging`
* :ref:`section_cmd_disable-logging`
* :ref:`section_cmd_load-configuration`
* :ref:`section_cmd_save-configuration`
* :ref:`section_cmd_close`
* :ref:`section_cmd_get-frame-count`
* :ref:`section_cmd_get-camera-guid`
* :ref:`section_cmd_get-status`
* :ref:`section_cmd_set-video-file`
* :ref:`section_cmd_get-video-file`
* :ref:`section_cmd_get-time-stamp`
* :ref:`section_cmd_get-frames-per-sec`
* :ref:`section_cmd_set-window-geometry`
* :ref:`section_cmd_get-window-geometry`
* :ref:`section_cmd_plugin-cmd`
* :ref:`section_cmd_set-camera-name`

.. _section_cmd_connect:

connect
-------

.. _section_cmd_disconnect:

disconnect
----------

.. _section_cmd_start-capture:

start-capture
-------------

.. _section_cmd_stop-capture:

stop-capture
------------

.. _section_cmd_get-configuration:

get-configuration
-----------------

.. _section_cmd_set-configuration:

set-configuration
-----------------

.. _section_cmd_enable-logging:

enable-logging
---------------

.. _section_cmd_disable-logging:

disable-logging
---------------

.. _section_cmd_load-configuration:

load-configuration
------------------

.. _section_cmd_save-configuration:

save-configuration
------------------

.. _section_cmd_close:

close
-----

.. _section_cmd_get-frame-count:

get-frame-count
---------------

.. _section_cmd_get-camera-guid:

get-camera-guid
---------------

.. _section_cmd_get-status:

get-status
----------

.. _section_cmd_set-video-file:

set-video-file
--------------

.. _section_cmd_get-video-file:

get-video-file
--------------

.. _section_cmd_get-time-stamp:

get-time-stamp
--------------

.. _section_cmd_get-frames-per-sec:

get-frames-per-sec
------------------

.. _section_cmd_set-window-geometry:

set-window-geometry
-------------------

.. _section_cmd_get-window-geometry:

get-window-geometry
-------------------

.. _section_cmd_plugin-cmd:

plugin-cmd
----------

.. _section_cmd_set-camera-name:

set-camera-name
----------------

