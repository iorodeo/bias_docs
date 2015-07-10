.. _section_bias_json_config:

***********************
BIAS JSON configuration 
***********************

BIAS' configuration data (GUI + camera settings) can be serialized to the json
format for saving/loading from configurations files, transmitting via http
requests, etc. A typical example of a serialize BIAS configuration is given
below  :ref:`section_bias_json_config_example`. Note, the exact details of the
serialized data will depend on the users specific setup i.e., camera type, GUI,
settings etc. The easiest way to get an example JSON configuration,  for your
specific setup, is generate a configuretion file via the "File -> Save
Configuration" menu item.

.. figure:: _static/bias_save_config_menu.png
   :align:  center


.. _section_bias_json_config_example:

BIAS JSON Configuration Example
-------------------------------

.. literalinclude:: _static/bias_config.json
   :language: javascript
