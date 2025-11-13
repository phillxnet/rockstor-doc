.. _immich_rockon:

Immich Rock-on
==========================

Before you install the Immich rock-on, you should understand the
prerequisites and configurations common to all Rockstor :ref:`rockons_intro`;
specifically the :ref:`rockons_preinstall` and :ref:`rockons_root`
requirements.


.. _immich_whatis:

What is Immich
---------------

`Immich <https://immich.app/>`_ is a free and open-source, self-hosted photo and 
video management solution. It allows you to easily back up, organize, and manage 
your personal media on your own server, ensuring that your privacy and data ownership 
are preserved. With Immich, you can upload photos and videos directly from your 
mobile devices, browse and search them with advanced tagging and metadata support, 
and view them in a modern, responsive interface. Immich also supports features 
such as automatic backup, multi-user accounts, EXIF data extraction, reverse geocoding, 
and AI-powered object detection and classification.
For further details and the most up-to-date description of Immich, please visit the 
`project's documentation <https://docs.immich.app/overview/quick-start/>`_.  

.. _immich_doc:

Immich Documentation
---------------------

Immich’s capabilities can be greatly expanded and customized to fit your storage 
setup, hardware, and preferences. The project provides extensive documentation 
to guide you through installation, configuration, and advanced features. 
Out-of-the-box, Immich already offers a wide range of functionality suitable for 
most users, including multi-device synchronization, map-based location views, 
and powerful search tools. Further customization—such as integrating with external 
storage, fine-tuning AI models, or extending server-side options is entirely optional 
but well supported.

.. _immich_install:

Installing the Immich rock-on
------------------------------
The Immich Rock-on requires 3 
`shares to be created <https://rockstor.com/docs/interface/storage/shares-btrfs-subvolumes.html#creating-a-share>`_
before the installation can begin:
 
 - Library share: used by Immich to store uploaded photos/videos,
 - Database share,
 - Cache share: used by Immich's machine-learning functionality

We are now ready to start the installation of the Immich rock-on. 

Click the *Install* button next to the **Immich** listing on the *Rock-ons*
page.

.. image:: /images/interface/docker-based-rock-ons/immich_preinstall.png
   :width: 100%
   :align: center


The install wizard has appeared. 

In the first step, please select the 3 shares that you created previously.

Then click on **Next**.

.. image:: /images/interface/docker-based-rock-ons/immich_shares.png
   :width: 100%
   :align: center


At this stage you will set the port used to reach Immich's web-UI. 

The default port is usually fine as long as it is free.

Then click on **Next**.
                       
.. image:: /images/interface/docker-based-rock-ons/immich_webUI_port.png
   :width: 100%
   :align: center

                       
At this stage you will set some environment variables.

**Log level**: Input the desired log level. You will want to use **log** 
for normal operations. Other available values: verbose, debug, warn, error.

**Ignore error checks**: Input whether you want to skip mount error checks. 
You will want to use **false**, unless you have 
`issues with your Immich instance <https://docs.immich.app/administration/system-integrity>`_.

Then click on **Next**.

.. image:: /images/interface/docker-based-rock-ons/immich_env_vars.png
   :width: 100%
   :align: center


Verify the information you've provided is correct, then click **Submit**.

.. image:: /images/interface/docker-based-rock-ons/immich_verify.png
   :width: 100%
   :align: center

                                   
You'll see a screen indicating the Rock-on is being installed. Click *Close*.

Congratulations! At this point Immich will be up-and-running. 

Click on the **Immich UI** button to open the web configuration and complete 
the Immich-specific set-up. (See: :ref:`_immich_setup`)

.. image:: /images/interface/docker-based-rock-ons/immich_afterinstall.png
   :width: 100%
   :align: center

                                   
.. _immich_external_library:

External Libraries
---------------

In case you already have a share full of existing photos and videos, you probably 
want to import them into Immich.
                                   
**WARNING: Write access!**

Mounting an external library of photos/videos via the Web-UI will give Immich 
write-access by default. 

If you want to restrict Immich to read-only-access, you will need to do the following instead:
                                   
 1. download the `Immich Rock-on configuration file <https://github.com/rockstor/rockon-registry/blob/master/immich.json>`_,
 2. save it under ``/opt/rockstor/rockons-metastore/``,
 3. edit the file: toward the end of the file, right after ``[ "-e", "DB_HOSTNAME=immich-database" ]``, insert the following line: ``, [ "-v", "/mnt2/MyPrivatePhotoCollection:/mnt2/MyPrivatePhotoCollection:ro" ]``, where ``/mnt2/MyPrivatePhotoCollection`` is the path to the share where you keep your existing photos/videos,
 4. save file,
 5. in case your Immich instance is already running: go into the "ROCK-ONS" menu, find the Immich Rock-on, set it to **OFF**, afterwards click on the **Uninstall** button,
 6. now re-install the rock-on.

                                   
**If you want to continue with a write-access external library, read on.**

To make the share available to Immich, you will need to first stop the running Immich instance.

You do that by clicking on the green **ON** button (which will subsequently turn to a red **OFF** button).

Now click on the small wrench icon right next to the red **OFF** button, and click on the **Add Storage** button.

.. image:: /images/interface/docker-based-rock-ons/immich_add_external_library.png
   :width: 100%
   :align: center

                                   
**Storage**: select the share that contains your existing photos/videos.

**Rock-on directory**: input a path under which your share will be available in the Immich instance.

Then click on **Next**.
                                   
.. image:: /images/interface/docker-based-rock-ons/immich_settings_for_external_library.png
   :width: 100%
   :align: center


If everything looks fine on the summary configuration page, then click on **Next** again, and then **Submit**.

After the Immich will be up-and-running again, go into your admin **External Libraries** configuration page in Immich, and add the supplied path (i.e. ``/data/test1``).

.. _immich_setup:

Finishing the Immich set-up
------------------------------

If you haven't already, click on the **Immich UI** button. At which point the web UI will load.

Click on the **Getting Started** button.

First you will create an admin account. 

If you intend to use the `email notification <https://docs.immich.app/administration/email-notification/>`_ feature, then it is recommended to use your actual email address. Otherwise, you can input any kind of email-like string into the *Admin Email* field.

Click on the **Theme** button and choose the preferred theme.

Click on the **Language** button and choose the preferred language.

Click on the **Server Privacy** button.

**Map**: if enabled, it will use Immich's map tile service to show the location of where the photo was taken on a world map.

**Version Check**: if enabled, it will periodically check Immich's github repo, if a new release is available.

Click on the **User Privacy** button.

**Google Cast**: if enabled, it will load resources from Google to make Google Cast work.

Click on the **Storage Template** button.

This setting is about how files are organized on the disk. If you want to keep it simple, keep it disabled. 
You can always migrate to storage templates at a later date.

But if you want custom define in what kind of folder structure and filenames you want your files to have,
then you should enable this setting.

You can also `check the detailed differences of using storage templates or not <https://docs.immich.app/administration/backup-and-restore#asset-types-and-storage-locations>`_.









