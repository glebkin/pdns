Conversion of old-style settings to YAML format
================================================

Running the command

.. code-block:: sh

   rec_control show-yaml

will show the conversion of existing old-style settings into the new YAML format.
The existing settings will be read from the default old-style settings file ``recursor.conf`` in the configuration directory.
It is also possible to show the conversion of a specific old-style settings file by running

.. code-block:: sh

   rec_control show-yaml path/to/recursor.conf

``rec_control show-yaml`` will also show the conversions of any included ``.conf`` file (if :ref:`setting-include-dir` is set) and other associated settings file, like :ref:`setting-forward-zones-file`.

Example
-------

Consider the old style configuration file ``recursor.conf``:

.. literalinclude:: example/recursor.conf

With the contents of ``recursor.d/01.conf``:

.. literalinclude:: example/recursor.d/01.conf

And ``fwzones.txt``:

.. literalinclude:: example/fwzones.txt

To show the conversion result, run:

.. code-block:: sh

   cd example
   rec_control show-yaml recursor.conf

Produces the following conversion report:

.. literalinclude:: example/conversion

Note  the ``!override`` tag for ``incoming.listen`` (corresponding to the ``=`` in ``recursor.d/01.conf``.
The  ``recursor.forward_zones`` settings is extending the setting in the main ``recursord.yml`` file, as ``recursor.d/01.conf`` uses a ``+=`` for the ``forward-zones`` settings.
Consult :doc:`../yamlsettings` for details on how settings spread over multiple files are merged.

The contents of the report can be used to produce YAML settings equivalent to the old-style settings.
This is a manual step and consists of copy-pasting the sections of the conversion report to individual files.
Any settings filename in the include directory should end with ``.yml``.
It is possible to put associated files (like a ``recursor.forward_zones_file``) into the include dir, but do *not* let these files end in ``.yml``, as ``.yml`` files will be interpreted as full configuration files, while the associated settings files are YAML sequences of a specific type.

API Managed Files
-----------------
The format of API managed files was also changed to use YAML format.
Specifically, the list of API managed zones is now a single file containing a sequence of ``auth_zones`` and a sequence of ``forward_zones`` instead of a settings file per zone.
The list of ACLs is a YAML sequence of subnets or IP addresses.
There is no conversion of API managed files from old-style to YAML yet.
The intention is to add automatic conversion code before the final release of a YAML enabled :program:`Recursor`,
