.. _neuropixelspxi:
.. role:: raw-html-m2r(raw)
   :format: html

################
Neuropixels PXI
################

.. image:: ../../_static/images/plugins/neuropix-pxi/neuropix-pxi-01.png
  :alt: Annotated Neuropixels PXI editor

.. csv-table:: Streams data from a PXI-based Neuropixels data acquisition system.
   :widths: 18, 80

   "*Plugin Type*", "Source"
   "*Platforms*", "Windows only"
   "*Built in?*", "No"
   "*Key Developers*", "Josh Siegle, Pavel Kulik"
   "*Source Code*", "https://github.com/open-ephys-plugins/neuropixels-pxi"

Installing and upgrading
############################

The Neuropixels PXI plugin is not included by default in the Open Ephys GUI. To install, use **ctrl-P** or **⌘P** to open the Plugin Installer, browse to the "Neuropixels PXI" plugin, and click the "Install" button. Alternatively, you can select the "Neuropixels" default configuration the first time the GUI is launched, and the plugin will be downloaded and installed automatically.

The Plugin Installer also allows you to upgrade to the latest version of this plugin, if it's already installed.

Hardware requirements
######################

* One **computer** (see :ref:`hardwarerequirements` for recommended specs)

* One **PXI chassis** (so far we've tested National Instruments PXIe-1071 and PXIe-1082, and ADLINK PXES-2301)

* One **PXI remote control module**, housed in the PXI chassis (we've tested National Instruments PXIe-8381 and PXIe-8398) – requires `NIDAQmx driver <https://www.ni.com/en-us/support/downloads/drivers/download.ni-daqmx.html>`__

* One **PCIe interface card**, housed in the computer (we've tested National Instruments PCIe-8381 and PCIe-8398)

* *(optional)* One **PXI-based analog and digital I/O module** (see the :ref:`NI-DAQmx` page for a list of hardware we've tested)

* **Cables** to connect the remote control module to the PCIe card (e.g., National Instruments MXI-Express Cables, Gen 2 x8)

* One or more **Neuropixels basestations** (available from IMEC)

* One or more **Neuropixels cables** (black + yellow twisted pair, USB-C to Omnetics, available from IMEC)

* One or more **Neuropixels headstages** (available from IMEC)

* One or more **Neuropixels probes** (available from IMEC)

.. warning:: Some users have reported not being able detect the PXIe remote control module, despite it being properly connected to the motherboard. In general, any physical x16 PCIe slot should be fine, but there are motherboards that contain x16 slots that are actually wired as x4, which won't work. There are also motherboards that have not been able to communicate with the PXI hardware for unknown reasons. If you are unsure whether your machine has the appropriate PCIe slots, please get in touch.


Compatible probes
######################

This plugin can stream data from the following Neuropixels probe types:

.. csv-table::
   :widths: 50, 40, 80

   "**Probe**", "**Channels**", "**Notes**"
   "Neuropixels 1.0", "384 AP, 384 LFP", ""
   "Neuropixels NHP Active", "384 AP, 384 LFP", ""
   "Neuropixels NHP Passive", "128 AP, 128 LFP", "May require :ref:`firmware update<Updating basestation firmware>`"
   "Neuropixels Ultra - Fixed", "384 AP, 384 LFP", ""
   "Neuropixels Ultra - Switchable", "384 AP, 384 LFP", "May require :ref:`firmware update<Updating basestation firmware>`"
   "Neuropixels 2.0 Single Shank", "384 wideband", "May require :ref:`firmware update<Updating basestation firmware>`"
   "Neuropixels 2.0 Four Shank", "384 wideband", "May require :ref:`firmware update<Updating basestation firmware>`"
   "Neuropixels Opto", "384 AP, 384 LFP", "Requires an Opto basestation"

Connecting to the PXI system
##############################

Before using this plugin, make sure you've followed all of the steps in the `Neuropixels User Manual <https://www.neuropixels.org/support>`__ to set up and configure your hardware. Prior to using your Neuropixels PXI basestation, you must install the Enclustra drivers (available for `Windows 7/8 <https://github.com/open-ephys-plugins/neuropixels-pxi/raw/main/Resources/Enclustra_Win7%268.zip>`__ and `Windows 10 <https://github.com/open-ephys-plugins/neuropixels-pxi/raw/main/Resources/Enclustra_Win10.zip>`__). See section 4.2.2 of the User Manual for installation instructions.

Once your PXI system is up and running, you can drag and drop the "Neuropix-PXI" module from the Processor List onto the Editor Viewport. The GUI will automatically connect to any available basestations in your connected PXI chassis. If no PXI basestations are found, the plugin can be run in :ref:`simulation mode<Simulation mode>`.

The editor will automatically create a probe selection interface for each basestation that's available. Each basestation can communicate with up to 4 probes (for Neuropixels 1.0, NHP, and Ultra) or 8 probes (for 2.0). When the probes are initially detected, they show up as orange circles. Once they are initialized, connected probes become green. After the probes turn green, the plugin is ready to begin data acquisition.

Calibrating probes
#####################

Neuropixels probes require ADC and gain calibration in order to function properly. These files can be obtained from IMEC for every probe that you've purchased. There should be two cvs files in one folder (<probe_serial_number>) for each probe:

* :code:`<probe_serial_number>_ADCCalibration.csv`

* :code:`<probe_serial_number>_gainCalValues.csv`

Any probes detected by the Neuropixels PXI plugin will be calibrated automatically when the plugin is loaded, provided that calibration folders are stored in one of the following locations:

* :code:`C:\\Program Files\\Open Ephys\\CalibrationInfo\\<probe_serial_number>` (recommended)

* :code:`<open-ephys-executable-folder>\\CalibrationInfo\\<probe_serial_number>` (if you used the Open Ephys installer, the executable will be located in :code:`C:\\Program Files\\Open Ephys`)

If these files cannot be found, a warning message will appear. It's still possible to acquire data from uncalibrated probes, but this data should be used for testing purposes only. The calibration files must copied to the correct location prior to running any actual experiments.

Configuring probe settings
###########################

To open the probe settings interface, press the "window" or "tab" button in the upper-right corner of the editor:

.. image:: ../../_static/images/plugins/neuropix-pxi/neuropix-pxi-04.png
  :alt: How to open the Neuropixels settings interface

Each probe has its own interface for updating settings, which is customized for each probe type. Selecting the green button corresponding to the probe's basestation and port in the plugin editor allows you to access the parameters for a particular probe. The button that is highlighted in light green indicates the probe whose settings are currently being viewed.

Here is an example of the settings interface for a Neuropixels 1.0 probe:

.. image:: ../../_static/images/plugins/neuropix-pxi/neuropix-pxi-02.png
  :alt: Overview of the Neuropixels 1.0 settings interface

And for a Neuropixels 2.0 (4-shank) probe:

.. image:: ../../_static/images/plugins/neuropix-pxi/neuropix-pxi-03.png
  :alt: Overview of the Neuropixels 2.0 settings interface

The interface on the left allows you to select/deselect electrodes from different banks. Use the mini probe overview visualization to scroll to the electrodes you want to activate, click or drag to select them in the zoomed visualization, and then click the "ENABLE" button. Selecting electrodes on one bank will automatically deactivate the electrodes on all other banks that are connected to the same set of channels.

In addition, for 1.0, NHP, and Ultra probes, you can change the following settings:

* **AP Gain** (amplifier gain for AP channels, 50x-3000x; default = 500x)

* **LFP Gain** (amplifier gain for LFP channels, 50x-3000x; default = 250x)

* **AP Filter Cut** (ON = 300 Hz high-pass filter active, OFF = filter inactive; default = ON)

Reference selection
###########################

All probe types include a **Reference** drop-down menu that can be used to select one of the following reference types:

* **External** (default) - references signals to the dedicated reference pad on the probe/flex cable. This pad can be connected to a wire immersed in saline above the brain (for acute recordings) or a screw embedded in the skull (for chronic recordings). It's common to connect the reference pad to the ground pad, to avoid the need for additional wires.

* **Tip** - references signals to the large pad at the tip of the probe (or the tip of a particular shank, in the case of the 4-shank Neuropixels 2.0). The tip reference will likely reduce your overall noise levels, but it will also lead to leakage of low-frequency signals across all channels. If you want to do any analysis of the local field potential, you need to be sure to keep at least a few channels outside the brain, in order to subtract their signals offline.

.. note:: As of GUI version 0.6.0, it's no longer possible to select the "Internal" reference channels of a Neuropixels probe. These channels are not suitable to use as a reference due to their high impedance.

In the Open Ephys GUI, reference settings are applied globally to all channels (i.e., you can't have a different gain for a subset of channels).

.. caution:: When using multiple PXI basestations in the same chassis, some users have reported problems with the External reference. This manifests as randomly occurring saturating events on the LFP channels, combined with a sudden drop in gain on the AP channels. Such events are not seen when using the Tip reference.

Activity view
###########################

Pressing the "VIEW" button in the "Probe Signal" area will toggle a live display of the amplitude range of each channel whenever acquisition is active. For Neuropixels 1.0 probes, activity can be viewed for the AP band or LFP band.

Saving, loading, and copying settings
######################################

Default loading and saving
---------------------------

Any changes made to the probe settings will be automatically re-applied when you re-start the GUI, provided you have checked **Reload on startup** from the "File" menu. Settings will first be transferred by probe serial number. If no matching serial number is found, settings will be inherited from a probe of the same type. Settings cannot be transferred between probes of different types (e.g. Neuropixels 1.0 to Neuropixels 2.0).

Copying settings between probes
--------------------------------
Settings can be transferred between probes using the "COPY", "PASTE", and "APPLY TO ALL" buttons:

.. image:: ../../_static/images/plugins/neuropix-pxi/neuropix-pxi-05.png
  :alt: Probe settings buttons

Settings can only be applied to probes of matching types (e.g. 1.0, NHP, Ultra, 2.0).

IMRO files
--------------------------------
Settings for individual probes can also be loaded using SpikeGLX "IMec Read Out" (IMRO) tables, using the "LOAD FROM IMRO" button. 

The IMRO format is specified `here <https://billkarsh.github.io/SpikeGLX/help/imroTables/>`__. If you've saved a probe configuration using SpikeGLX or some other software, you can apply that configuration to a probe in the Open Ephys GUI by reading in an IMRO file. The only caveat is that Open Ephys does not allow individual channels to have different gain or reference settings, so those will be inherited from the last channel in the file.

You can save the configuration for a particular probe into IMRO format using the "SAVE TO IMRO" button. These files can be used in SpikeGLX or any other software that can read the IMRO format.

Any IMRO files that have been loaded previously will appear in the drop-down menu below the "LOAD FROM IMRO" button, so they can be accessed more easily.

ProbeInterface JSON files
--------------------------------

If you're performing offline analysis with `SpikeInterface <https://github.com/spikeinterface/spikeinterface>`__, it may be helpful to have information about your probe's channel configuration stored in a JSON file that conforms to the `ProbeInterface <https://github.com/spikeinterface/probeinterface>`__ specification. To export a ProbeInterface JSON file, simply press the "SAVE TO JSON" button.

Plugin data streams
######################################

The Neuropixels PXI plugin sends data from all connected probes through the GUI's signal chain. To disable data transmission, a probe needs to be physically disconnected from the basestation. The plugin should be deleted and re-loaded any time a probe is connected or disconnected.

If you're using Neuropixels 1.0, NHP, or Ultra probes, each probe will have two data streams: 

* 384 channels of AP band data, sampled at 30 kHz

* 384 channels of LFP band data, sampled at 2.5 kHz. 

If you're using Neuropixels 2.0 probes, each probe will have only one data stream:

* 384 channels of wide-band data, sampled at 30 kHz.

As of GUI version 0.6.0, settings for each stream are configured independently for each stream. This makes it much easier to apply different parameters to different streams, for example unique filter settings for the AP band and LFP band. However, users should be aware that settings for one stream are not automatically applied to other streams. If you are recording from many probes simultaneously, be sure to use the Stream Selector interface in downstream plugins to confirm that the appropriate settings have taken effect for all incoming data streams.

Customizing stream names
--------------------------

Clicking on the slot number for a given basestation will open up an interface for customizing the names of the data streams generated by the Neuropixels PXI plugin. By default, each probe is assigned a name based on the order that it's detected: :code:`ProbeA`, :code:`ProbeB`, :code:`ProbeC`, etc. While this is fine for most use cases, there are some situations where other behavior is desirable. Therefore, the plugin includes four different schemes for naming data streams, which can be applied independently for each basestation:

.. image:: ../../_static/images/plugins/neuropix-pxi/neuropix-pxi-07.png
  :alt: Four different stream naming interfaces

#. **Automatic naming:** Probes names are assigned automatically, based on the order in which they are detected. Any 1.0 probes will have "-AP" and "-LFP" appended to their respective streams. The naming interface displays the names that will be applied when using this scheme, but they cannot be edited.

#. **Automatic numbering:** Numeric stream names are assigned automatically, based on the order in which they are detected. This scheme will produce file names that look like those from GUI version 0.5.X and earlier, which did not have the ability to apply custom names to individual streams. The naming interface displays the names that will be applied when using this scheme, but they cannot be edited.

#. **Custom port names:** Probe names are assigned by port/dock. This is useful if you have probes placed in a particular physical configuration, and always want a probe in a certain position to have the same name, regardless of which other probes are connected.

#. **Custom probe names:** Porbe names are assigned by serial number. This is useful if you have probes chronically implanted and would like to associate the subject ID with a particular probe.

.. caution:: All stream names *must* be unique for a given plugin. Currently, it's possible to inadvertently assign the same name to multiple probes, either by using the same port-specific or probe-specific names across basestations. Name conflicts must be checked manually in order to prevent crashes when starting recording.

Synchronization settings
######################################

Properly configuring your synchronization signals is critical for Neuropixels recordings. Each probe will have a slightly different sample rate between 29999.9 and 30000.1 Hz, so you cannot simply count samples to figure out how much time has elapsed for a given data stream. Therefore, every data source (including individual basestations, NI hardware, etc.) must share a hardware sync line in order for samples to be accurately aligned offline.

Each Neuropixels basestation contains one SMA connector for sync input. The behavior of these connectors is configured using the synchronization interface within the plugin editor:

.. image:: ../../_static/images/plugins/neuropix-pxi/neuropix-pxi-06.png
  :alt: Updating sync settings

* The top drop-down menu allows you to select one basestation's SMA connector to serve as the "main" sync. The signal on this line will be automatically copied to the sync inputs of all other basestations.

* The "+" button allows you to toggle whether or not the sync line is appended to all data streams as a continuous channel. When this button is orange, each stream will include a 385th channel containing the state of the sync line. This will make the :ref:`binaryformat` data files saved by the Record Node compatible with a variety of SpikeGLX-associated offline processing tools, such as CatGT. This button should be enabled *only* if you plan to use these tools. Regardless of whether or not this option is enabled, the sync rising and falling edges will be transmitted as events to downstream processors.

* The second drop-down menu allows you to configure the main sync SMA as **INPUT** or **OUTPUT**. In **INPUT** mode, an external digital input must be connected to the SMA. In **OUTPUT** mode, the master basestation will generate its own sync signal at 1 Hz or 10 Hz. 

Simulation mode
##############################

When running the plugin in simulation mode, you'll have the option of selecting up to four different probes to acquire data from. This is useful for familiarizing yourself with the settings interfaces for different probe types, or testing your signal chain in the absence of any Neuropixels hardware.

The simulated AP band data was designed to make the probe activity view look interesting; the simulated LFP band data is sine waves with amplitudes that vary across channels.

Built-in self tests
#####################

If you have a probe that's not working properly, these tests can be used to help pinpoint where the problem lies. It's not recommended to run the tests prior to every recording; the tests are only necessary to diagnose an issue with a probe that is not transmitting data.

To run each test, select one from the drop-down menu, and click the "RUN" button. After the test completes, the name of the test will be updated to indicated whether it passed or failed.

.. csv-table:: Built-in self tests
   :header: "Name", "Duration", "Purpose"
   :widths: 20, 20, 70

   "Test probe signal",	"30 s", "Analyzes if the probe performance falls within a specified tolerance range, based on a signal generated by the headstage"
   "Test probe noise", "30 s", "Calculates probe noise levels when electrode inputs are shorted to ground"
   "Test PSB bus", "<1 s", "Verifies whether signals are transmitted accurately to the headstage"
   "Test shift registers", "1 s", "Verifies the functionality of the shank and base shift registers"
   "Test EEPROM", "1 s", "Tests the EEPROM memory storage on the flex, headstage, and BSC"
   "Test I2C", "<1 s", "Verifies the functionality of the I2C memory map"
   "Test Serdes", "<1 s", "Tests the integrity of the serial communication over the probe cable"
   "Test Heartbeat", "3 s", "Tests whether the heartbeat signal between the headstage and BSC is working properly"
   "Test Basestation", "<1 s", "Tests the BSC board"

.. note:: If the "probe signal" and "probe noise" tests fail, it does not necessarily indicate that the probe is broken. If your probe is successfully transmitting data, the outcome of these tests can be ignored.

Headstage tests
#################

If you have a headstage test module, you can run a suite of tests to ensure the headstage is functioning properly. When the Neuropix plugin is dropped into the signal chain and at least one headstage test module is connected to the PXI system, the GUI will automatically run all headstage tests and output the results in a popup window:

.. image:: ../../_static/images/plugins/neuropix-pxi/HST.png
  :alt: Headstage test board popup window
  :width: 400

.. note:: The headstage test module will only work if you have *not* updated your basestation firmware. However, we have also found that the headstage tests are rarely needed to accurately diagnose a problem with data transmission. If you are unsure whether your headstage is functional, swapping it out with a different headstage is usually more informative than running the headstage tests.

Updating basestation firmware
######################################

This plugin is compatible with any recent basestation firmware version. However, if you're using Neuropixels 2.0, NHP Passive, or switchable Ultra probes, you'll need to upgrade to the latest firmware (available `here <https://github.com/open-ephys-plugins/neuropixels-pxi/raw/main/Resources/Neuropixels_PXI_APIv3_Firmware.zip>`__).

The currently installed firmware version will appear in the info section of the Neuropixels settings interface (upper right text block). If your basesation firmware version is "2.0137" and your basestation connect board firmware version is "3.2176", you already have the latest firmware installed.

.. note:: If you've been using SpikeGLX version 3.0 or higher, your basestation already has the latest firmware.

If you need to update your firmware, first click the "UPDATE FIRMWARE" button to open the firmware update interface:

.. image:: ../../_static/images/plugins/neuropix-pxi/neuropix-pxi-08.png
  :alt: Interface for updating firmware

Next, select a :code:`.bin` file for the basestation connect board (:code:`QBSC*.bin`), and click "UPLOAD". The upload process can take anywhere from 10-15 minutes, so please be patient.

Immediately after the basestation connect board firmware upload finished, use the lower drop-down menu to select a :code:`.bin` file for the basestation (:code:`BS*.bin`), and click "UPLOAD". 

Finally, once the basestation firmware is finished uploading, restart your computer and power cycle the PXI chassis for the changes to take effect.

.. note:: If you need to update the firmware for multiple basestations in one chassis, please perform all firmware updates prior to restarting your chassis/computer. Alternatively, you can update each basestation separately if only one basestation at a time is inserted into the chassis. The Neuropixels plugin can only communicate with sets of basestations that are running the same firmware.
