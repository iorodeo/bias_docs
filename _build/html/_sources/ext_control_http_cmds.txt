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
(possibly empty).  In this example the "localhost" is used for the devie
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
Connect to the camera which is associated with the given BIAS camera window
(specified by port address).

.. code-block:: bash 

   http://localhost:5010/?connect


.. _section_cmd_disconnect:

disconnect
----------
Disconnect from the camera which is associated with the given BIAS camera
(specified by port address).

.. code-block:: bash 

   http://localhost:5010/?disconnect


.. _section_cmd_start-capture:

start-capture
-------------
Starts image capture on the associated camera. Note, the camera must be
connected for the command to be successful. 

.. code-block:: bash 

   http://localhost:5010/?start-capture

Note, the current connection status of the camera can be queried using
:ref:`section_cmd_get-status` command.

.. _section_cmd_stop-capture:

stop-capture
------------
Stops image capture on the associated camera. 

.. code-block:: bash 

   http://localhost:5010/?stop-capture

.. _section_cmd_get-configuration:

get-configuration
-----------------
Returns the current BIAS configuration in the json format (all GUI and camera settings).

.. code-block:: bash 

   http://localhost:5010/?get-configuration

An example of the typical data returned by this command can be found here
:ref:`section_bias_json_config_example`. Note, the camera must be connected for
this command to be successful.


.. _section_cmd_set-configuration:

set-configuration
-----------------
Sets the configuration of the associated GUI window and camera (specified by
port address) to that given by the json configuration provided in the command
argument.

.. code-block:: bash

    http://localhost:5010/?set-configuration=<json configuration object>

Note, the the camera must be connected for this command to be successfull. In
addition the configuration data must be valid for the users camera make and
model.  An example of setting the configuration for a Point Grey Flea3
FL3-U3-13Y3M camera is shown  below.  

.. literalinclude:: _static/set_config_http_example.txt 
   :language: bash
    

.. _section_cmd_enable-logging:

enable-logging
---------------
Enable logging of video data to data file. This command is equivalent to
checking the "Logging -> enabled" checkbox. 

.. code-block:: bash

    http://localhost:5010/?enable-logging

Note, logging cannot be enabled during image capture. In order to enable
logging the current capture (if running) must be stopped - via the
:ref:`section_cmd_stop-capture` command or equivalent. Then logging may be
enabled and capture resumed.  Attempting to enable logging during capture will
return an error as follows:

.. code-block:: javascript

    { 
        "command" : "enable-logging", 
        "message" : "Unable to enable logging: capturing images", 
        "success" : false, 
        "value"   : "" 
    }

.. _section_cmd_disable-logging:

disable-logging
---------------
Disable logging of video data. This command is equivalent to unchecking the
"Logging -> enabled" checkbox.

.. code-block:: bash

    http://localhost:5010/?disable-logging

Note, logging cannot be disabled if image capture has been started. In order to
disable logging it is necessary to first capturing images via the
:ref:`section_cmd_stop-capture` command or similar. Attempting to disable
logging during capture will return an error as follows:

.. code-block:: javascript

    { 
        "command" : "disable-logging", 
        "message" : "Unable to disable logging: capturing images", 
        "success" : false, 
        "value"   : "" 
    }



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

