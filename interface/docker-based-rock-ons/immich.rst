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

                                   
.. _immich_mounting_external_library:

Mounting External Libraries
---------------

In case you already have a share full of existing photos and videos, you probably 
want to import them into Immich. 
You can `read more about them <https://docs.immich.app/features/libraries>`_.
                                   
**WARNING: Write access!**

Mounting an external library of photos/videos via the Web-UI will give Immich 
write-access by default. This also means you will be able to delete files via Immich.

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

If you intend to use the `email notification <https://docs.immich.app/administration/email-notification/>`_ feature, 
then it is recommended to use your actual email address. 
Otherwise, you can input any kind of email-like string into the *Admin Email* field.

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

But if you want to custom define what kind of folder structure and filenames you want to have,
then you should enable this setting.

You can also 
`check the detailed differences of using storage templates or not <https://docs.immich.app/administration/backup-and-restore#asset-types-and-storage-locations>`_.

Click on the **Backups** button and consider if you have any special backup requirements for your Immich instance.

Click on the **Mobile App** button and consider if you want to install their official mobile app.

Having the mobile app will allow you to access your files, 
but more importantly, you will also be able to set automatic, background uploading of photos/videos to your Immich instance.

Click on the **Done** button to finish the set-up.

.. _immich_new_user:

New user account
------------------------------

If you are the only user, or if you fully trust others, you can simply use the Admin account.
Otherwise, you can consider creating new user accounts.

Login to your Immich instance via a browser with your Admin account.

Click on your profile image on the top-right corner of the page, then click on **Administration**.
                                   
.. image:: /images/interface/docker-based-rock-ons/immich_administration.png
   :width: 100%
   :align: center


Click on the **Create user** link below your profile image on the top-right corner of the page.

.. image:: /images/interface/docker-based-rock-ons/immich_create_user.png
   :width: 100%
   :align: center


Fill in the user account data. Note that files coming from external libraries do *not* count toward the Quota Size.

.. image:: /images/interface/docker-based-rock-ons/immich_user_form.png
   :width: 100%
   :align: center


Click on the **Create** button to create the new user account.

.. _immich_external_library_import:

External Library Import
------------------------------

If you have an existing collection of photos and videos, you are probably going to want to include it into your Immich instance.

**Note that there is currently no good central sharing solution present in Immich!**
If you wanted to have a central repository of photos/videos that is shared between different users, 
`you will need to wait <https://github.com/immich-app/immich/issues/12614>`_ until it's implemented.

Until then, here are some alternatives:
 1. create a single user account that you share with other people,
 2. 

Prerequisite is that you have made your share available to Immich. See: :ref:`_immich_external_library_setup`.

Click on your profile image on the top-right corner of the page, then click on **Administration**.
                                   
.. image:: /images/interface/docker-based-rock-ons/immich_administration.png
   :width: 100%
   :align: center


Click on the **External Libraries** item in the left-side menu.
                                   
.. image:: /images/interface/docker-based-rock-ons/immich_external_libraries.png
   :width: 100%
   :align: center


Click on the **Create library** link below your profile image on the top-right corner of the page.
                                   
.. image:: /images/interface/docker-based-rock-ons/immich_create_library.png
   :width: 100%
   :align: center


You can read an `in-depth explanation and troubleshooting information <https://docs.immich.app/features/libraries>`_.
