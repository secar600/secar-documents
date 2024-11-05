
Data Acquisition
================

Instructions for Users
----------------------

General Layout
~~~~~~~~~~~~~~

SECAR has two data acquisition systems that are connected with each other to create a common data stream, one for the focal plane and one for the target. Each system consists of an spdaq machine (:code:`spdaq57` for the focal plane and :code:`spdaq58` for the target), which is located in the vault and directly connected to Pixie-16 crates. The creates include digitizer cards with up to 16 channels each where detector and other signals are plugged into. A channel map file :code:`/daq/daq_map_file.csv` lists crates, modules, channels, and what is connected to them.

.. warning::

 Do not run anything unnecessary on the spdaq machines. Unless indicated otherwise, everything discussed in this section should be run on any data U machine connected to the DAQ network.

All DAQ operations should be done on the DAQ network PCs in the DATAU. See display labels to make sure you are using the correct PC.

After logging in on the experiment account on a DAQ network machine and opening a terminal you can

- start the readout code, which can be used to start data taking and writing data to disk,
- the scaler code which starts the scalers for each channel that are also added to the data stream,
- SpecTcl, which is used for online monitoring of either incoming data, or data that have already been written to disk, and which allows you to look at various spectra.

If you need to setup data acquisition parameters or trouble shoot the data acquisition you can use qtscope. This needs to be run on the respective spdaq machine (ssh tunneling is setup to run automatically, so use the goqtscope_##.sh scripts only).

Start Readout
~~~~~~~~~~~~~

#. Logon to the experiment account on a computer connected to the DAQ network (label on monitor says DAQ)
#. Open a terminal
#. Enter :code:`goReadout.sh`
#. Click on :code:`Start` button to initialize. Two tabs appear for the two spdaq machines - wait until you see "Scalers know crate ID=0" messages - there is 1 message for each digitizer card, so there will be 2 for spdaq 58 (target) and 5 for spdaq 57 (focal plane)
#. You can now start a run by entering a title and clicking on :code:`Begin`. You should start the scalers first - see next section. You need to select :code:`Record` to write data to disk.

Start Scalers
~~~~~~~~~~~~~
#. Open a terminal
#. Enter :code:`goscalers.sh`
#. Note for experiments with SiLi detector: it is plugged into Si1 (the first of the two scattering monitor silicon channels

Start SpecTcl
~~~~~~~~~~~~~
#. Open a terminal
#. Enter :code:`goSpecTcl.sh`
#. First, load the standard Definitions file basics_definitions.tcl in the treegui window by clicking the load button.
#. Next, load the desired window configuration in Xamine Window->Read Configuration
#. Note for experiments with SiLi detector: parameter is SiAScat0 and spectrum SiLi.energy

Start qtscope
~~~~~~~~~~~~
#. Open a terminal
#. start qtscope by entering :code:`goqtscope_FP.sh` or :code:`goqtscope_TAR.sh` for focal plane or target system, respectively
#. click on :code:`Boot`

Start nscope (legacy software)
~~~~~~~~~~~~
#. Open a terminal
#. start nscope by entering :code:`gonscope_FP.sh` or :code:`gonscope_TAR.sh` for focal plane or target system, respectively
#. click on :code:`Boot`

.. warning::

 The following are notes for experts that are not up to date

Digitizers
----------

SECAR has two Pixie-16 electronics crates:

- :code:`spdaq57` is the Pixie-16 crate for focal plane 4 of SECAR, where the end detectors are connected to.
- :code:`spdaq58` is the Pixie-16 crate located near the target area of SECAR, where the JENSA detectors are connected to.

The JENSA crate (:code:`spdaq58`) has two digitizer cards with 16 channels each. 26 BGO detectors, two PIPS detectors, a pulser, EBIT and an RF signal are connected to these cards. Focal plane crate (:code:`spdaq57`) has 5 digitizer cards with 16 channels each. Two MCP detectors (5 signals per each MCP), ionization chamber, DSSD detector (32+32 strips = 64 channels), and a pulser are connected to these cards.

There are two map files, one for the JENSA crate and one for the focal plane crate, in which the channels and slots of SECAR digitizers are assigned to all SECAR's detectors' signals. These files are automatically accessed by the DAQ and scalers to keep track of where each signal was acquired from when we save runs. The DAQ saves a master map file in the elog when a run is taken. This way, for each test or experiment, one can keep track of where each detector signal was connected to.

.. tip::

       **Please make sure you go through the master map file (a csv file) prior to each experiment and ensure the maps of each digitizer channel and slot corresponds to where each signal is connected to on the digitizer cards.**

The map files are part of the DAQ and scalers git repository, as well as the Python offline analysis package that Caleb Marshall and I have developed for SECAR.

.. warning::

  Slot #5 in the focal plane 4 Pixie-16 crate is broken. It has some weird issues and causes warning messages about the detectors that are in that slot when NSCL DAQ is running. After taling to Giordiano and Aaron, we could not really find out what exactly is wrong with this slot. So, they recommended that this slot be skipped. This is why no card is attached to this slot. Please avoid using this slot.

Readout Shell (NSCL DAQ)
------------------------

To be able to successfully run NSCL DAQ (readoutshell), log into your desired active user account from a computer on the DAQ network. Only those active user accounts to which an stagearea is assigned by the business IT department can run the DAQ. If this is not the case, and the user account is too old and/or for some reason the stagearea is not assigned to it anymore, one cannot save new runs and run the DAQ using those accounts.

Assuming you have an active user account with a valid stagearea, open a terminal on a computer connected to the DAQ network. Follow the instructions below to set up the DAQ for the first time:

.. code-block::
  :caption: How to set up Readoutshell (NSCL DAQ) for the first time

       cd ~
       # If the DAQ repository does not exist:
               git clone --recursive https://git.frib.msu.edu/secar/daq.git (--recursive is due to the fact that elog repository is also inside the DAQ repository)
       # If the DAQ repository already exists:
               cd ~/daq
               git pull
               # Follow the prompt
       emacs ~/stagearea/.settings.tcl
       # Make sure DAQ version in this file (the number that comes after nscldaq) is the same as the one in ~/daq/goReadout.sh.
       # If not, change the DAQ versions in ~/stagearea/.settings.tcl file to match those of the ~/daq/goReadout.sh file.
       # When running multi-crate mode, make sure the sourceid parameter is specified in the .settings.tcl file BOTH inside the parameters section (needs :code:`-sourceid 0` or :code:`1`) and after the parameters section (needs :code:`sourceid 0` or :code:`1` with no :code:`-`.) 0 or 1 here corresponds to the crate sourceid. For our configuration spdaq58 is source 0 and spdaq57 is source 1.
       cd ~/daq
       # Make sure RunMeFirst.sh and RunMeNext.sh have executable permissions (chmod +x RunMeFirst.sh) and (chmod +x RunMeNext.sh)
       ./RunMeFirst.sh
       # Input your desired user account on the prompt
       ./RunMeNext.sh

At this point the DAQ window opens. Click on "Start" and monitor the two windows that show :file:`SSHPipe@spdaq57:1` and :file:`SSHPipespdaq58:0`. They should run free of errors. :file:`SSHPipe@spdaq57:1` should restart 5 digitizers and by the end of this process, you should see 5 lines that read :code:`date/time : output : SSHPipespdaq57:1: Scalers know crate ID = 1`. Similarly, :file:`SSHPipe@spdaq58:0` should restart 2 digitizers and by the end of this process, you should see 2 lines that read :code:`date/time : output : SSHPipespdaq58:0: Scalers know crate ID = 0`. The :file:`main` window should also indicate no errors. If this is the case, then make sure "Record" is selected and if you like to time the run, select "Timed Run" and choose how long you want the run to be. Pick a good title for the run and click on "Begin". Now, the DAQ window turns green and data are being saved into the disk.

All DAQ ring sources should be automatically created you start readout for the first time. The event builder output ring will generate the first time you start a run in readout. These rings include:
- 2 raw data buffers (1 for each crate), run on each spdaq machine. These are named :code:`crate_1_raw` and :code:`crate_2_raw`.
- 2 sort buffers (1 for each crate) run on daqcompute002 node. These are named :code:`crate_1_data` and :code:`crate_2_data`.
- 1 and the event built output ring (run on the local u5pc4 DATAu computer).

To check that the rings are created and running correctly, from the DATAu computer run :code:`$DAQBIN/ringbuffer status` where :code:`$DAQBIN` is :code:`/usr/opt/daq/DAQ-VERSION-HERE/bin`. You should see the list of rings and the free size should match the data-size if there is no run active. If the free is stuck at 0, the ring will need to be deleted and re-created and the daq computers restarted. This can happen if too much data is read out to the ring and the buffer is ever full. This causes a backwards pileup that can crash the DAQ computers. See the NSCL DAQ manual pages for more information.

As of beam time in November 2024, we are using DAQ-VERSION 12.0-021. All files that reference a DAQ version should reflect the correct version number you are using.

If you get any error, please refer to the troubleshooting subsection. In general, most of the times, you unfortunately end up fixing DAQ issues by restarting the crates multiple times. In order to restart the crates, turn JENSA crate OFF first, then turn OFF focal plane 4 crate and restart the JENSA crate first followed by the crate at focal plane 4. The JENSA crate gives the master clock signal to the combined system and must be started first for the two crate system to work correctly.


To make sure everything looks good, run the DAQ and then open a terminal and type:

.. code-block::
  :caption: Ensuring Readoutshell (NSCL DAQ) is running fine

       cd ~
       startev
       $DAQBIN/dumper -s tcp://localhost/e20008_evb -c 50
       $DAQBIN/ringbuffer status

If $DAQBIN is not being set, run :code:`startev`, then replace $DAQBIN in the above commands with :code:`/usr/opt/daq/DAQ-VERSION-HERE/bin`.

The output should look like :numref:`dumper` and :numref:`status` screenshots. Once the DAQ is up and running, you can ensure the master ring is alive and well by typing in the terminal:

.. code-block::
  :caption: Ensuring Readoutshell (NSCL DAQ) master ring is running fine

       startev
       telnet u5pc4 30000 # (where u2pc4 is the localhost where the master ring is being built from)

If the DAQ is already set up and you just want to run it, do the following:

.. code-block::
  :caption: How to run Readoutshell (NSCL DAQ)

       goReadout.sh

Once you clone the DAQ repository, you will also have access to qtscope and a few other scripts which set up bashrc and the correct Debian environment. Also, the automatic elogs that are generated with each run saved by the DAQ and the files that get attached to the elog entries are already set up once you follow the steps above. You do not need to copy/paste anything or change the paths manually.

If you would like to change the version of the NSCL DAQ you are running, you need to do the following: clone the DAQ directory (see the beginning of this section). Then, do the following:

.. code-block::
  :caption: How to update Readoutshell (NSCL DAQ) version

       startev
       emacs stagearea/.setting.tcl
       # Find and replace all instances of 11.3-029 to the newer version
       # Save the changes
       emacs ~/goReadout.sh
       # Change all instances of 6.1-001 (for ddas) to the desired new version of ddas firmware
       # Change all instances of 11.3-029 to the desired new version of DAQ
       # Save the changes
       cd ~/scalers/fp
       emacs goscaler_fp
       # Change all instances of 11.3-029 to the desired new DAQ version
       # Save changes
       cd ../jensa/
       emacs goscaler_jensa
       # Change all instances of 11.3-029 to the desired new DAQ version
       # Save changes

.. _dumper:
.. figure:: Figures/dumper.jpeg
  :scale: 70%

  Once DAQ is running, run the :code:`$DAQBIN/dumper -s tcp://localhost/e20008_evb -c 10` command to ensure that everything is OK. The output of this command should be like the image above.

.. _status:
.. figure:: Figures/status.jpeg
  :scale: 70%

  Once DAQ is running, run the :code:`$DAQBIN/ringbuffer status` command to ensure that everything is OK. The output of this command should be like the image above.

Setting Up Required Files for DAQ
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

There are 4 files which generally require some level of customization for a new daq setup. These are
#. cfgPixie16.txt
#. modevtlen.txt
#. .settings.tcl
#. ReadoutCallouts.tcl

Detailed information about these files and how to set them can be found in the NSCL DAQ manuals. If you get errors while running the NSCL DAQ, it may be because you have not set these files correctly.

cfgPixie16.txt
--------------

For :code:`spdaq58` crate (located near the SECAR's target area), you will find the correct file under :code:`~/daq/readout/crate_1/` directory and its content looks like:

.. code-block::
  :caption: The content of :code:`spdaq58:~/daq/readout/crate_1/cfgPixie16.txt` file

       0  #Crate ID
       2  #number of modules
       2  #slot for mod 0
       3
       /user/e20008/daq/readout/crate_1/crate_1.set

For :code:`spdaq57` crate (located at focal plane 4), you will find the correct file under :code:`~/daq/readout/crate_2/` directory and its content looks like:
.. code-block::
  :caption: The content of :code:`spdaq57:~/daq/readout/crate_2/cfgPixie16.txt` file

       1  #Crate ID
       5  #number of modules
       2  #slot for mod 0
       3
       4
       6
       7
       /user/e20008/daq/readout/crate_2/crate_2.set

**Here, slot number 5 is skipped because it is broken. Please keep this file as is.**

The last line of each file shows the path to the binary set file should be used by DAQ: :code:`crate_1.set` and :code:`crate_2.set`.

The binary set files are saved via :code:`qtscope` when the DDAS parameters (i.e.energy and timing filters, etc.) are being set by the user. If you desire to use another set file for either crate, save a new .set file with the name you want and change this file path in the corresponding :code:`cfgPixie.16.txt` file, (e.x. /user/e20008/readout/crate_1/crate_1_new_settings.set).

modevtlen.txt
-------------

The content of the :code:`spdaq58:~/daq/readout/crate_1/modevtlen.txt` and :code:`spdaq57:~/daq/readout/crate_2/modevtlen.txt` files depends on two factors:

- How many digitizer cards are in each of these crates.
- Whether or not traces and QDCs are turned ON.

We typically use 5 digitizer cards in the :code:`spdaq57` crate (4 for DSSD, 1 for IC, MCPs, etc.); and 2 digitizer cards in the :code:`spdaq58` crate. Therefore, when traces are OFF, you should see:

.. code-block::
  :caption: The content of :code:`spdaq58:~/daq/readout/crate_1/modevtlen.txt` file when traces are OFF

      4
      4

.. code-block::
  :caption: The content of :code:`spdaq57:~/daq/readout/crate_2/modevtlen.txt` file when traces are OFF

      4
      4
      4
      4
      4

When the traces are turned ON, you need to change the above mentioned :code:`4` values to a different number **only for the digitizer card for which traces are turned ON**. The new value is calculated from this formula: :math:`Trace\,length / 8 + 4`, where trace length is in :math:`{\mu}s` and can be changed using the :code:`Pulse shape` feature provided by :code:`qtscope`, and :math:`Trace\,length / 8` should be an integer number.

.. code-block::
  :caption: The content of :code:`spdaq57:~/daq/readout/crate_2/modevtlen.txt` file when traces are ON for the first digitizer card containing MCPs

      129
      4
      4
      4
      4

where :code:`129` is calculated based on a trace length of :math:`1\,{\mu}s`: :math:`1000 / 8 + 4 = 129`. In this example, the traces are only ON for the first digitizer card containing MCPs.

If QDCs are turned ON, you will need to add 8 to the result and **only for those cards for which the QDC features are turned ON**. There are 8 QDCs in total. So, if you decide to turn them ON for the same card that contains the MCPs, instead of 129, you need to enter :math:`129 + 8 = 137`. If traces are turned OFF for this card and only QDCs are turned ON, the correct number that should be entered to the first line of the :code:`spdaq57:~/daq/readout/crate_2/modevtlen.txt` file would be :math:`4 + 8 = 12`.

.settings.tcl
-------------

The .settings.tcl file should be located in :code:`~/stagearea/.settings.tcl` and should include the following:
.. code-block::
  :caption: Replace 12.0-021 with your DAQ version and e20008 with your experimental account.

        set EventLogger /usr/opt/daq/12.0-021/bin/eventlog
        set EventLoggerRing tcp://localhost/e20008_evb
        set EventLogUseNsrcsFlag 1
        set EventLogAdditionalSources 0
        set EventLogUseGUIRunNumber 1
        set EventLogUseChecksumFlag 1
        set EventLogRunFilePrefix run
        set StageArea /user/e20008/stagearea
        set run 100
        set title test_run_title
        set recording 0
        set timedRun 0
        set duration 3600
        set dataSources {{host spdaq58.nscl.msu.edu parameters {-readouthost spdaq58.nscl.msu.edu -readoutring crate_1_raw -sorthost daqcompute002.frib.msu.edu -sortring crate_1_data -cratedir /user/e20008/daq/readout/crate_1 -sourceid 0} path /usr/opt/daq/12.0-021/bin/ddasReadout provider SSHPipe sourceid 0 wdir /user/e20008/daq/readout/crate_1} {host spdaq57.nscl.msu.edu parameters {-readouthost spdaq57.nscl.msu.edu -readoutring crate_2_raw -sorthost daqcompute002.frib.msu.edu -sortring crate_2_data -cratedir /user/e20008/daq/readout/crate_2 -sourceid 1} path /usr/opt/daq/12.0-021/bin/ddasReadout provider SSHPipe sourceid 1 wdir /user/e20008/daq/readout/crate_2}}
        set segmentsize 1000000

The most important part of this file is the dataSources section. Double check that:
- -readouthost and -readoutring should match the spdaq and raw data buffer you are using
- -sorthost and -sortring should match the machine doing the time sorting (usually daqcompute002 or another available daqcompute node)
- -sourceid 0 for spdaq58 and -sourceid 1 for spdaq57. Make sure you also give :code:`sourceid 0` and :code:`sourceid 1` after the parameters section.
- -cratedir should match the directory where the crate .set file, cfgPixie16.txt and modevtlen.txt files are located.

ReadoutCallouts.tcl
-------------------

Most of the code in this file creates methods to automatically post to the elog on run start/end. The DAQ setup information that is important is at the end of this file where the ring sources are registered for the event builder. The relevant lines should be:
.. code-block::
    EVBC::registerRingSource tcp://daqcompute002.frib.msu.edu/crate_1_data "" 0 {crate_1_data} 1 1 5 0
    EVBC::registerRingSource tcp://daqcompute002.frib.msu.edu/crate_2_data "" 1 {crate_2_data} 1 1 5 0

The 0 and 1 after the double "" quotes should correspond to the sourceids for each crate.
You can add other custom .tcl code in ReadoutCallouts that will be run when runs begin/end etc. to provide additional diagnostic information or automate other processes.


Troubleshooting DAQ
~~~~~~~~~~~~~~~~~~~

If the DAQ crashes and when you try to run it again, you get an error complaining about ringbuffer (the error indicates that another active ringbuffer already exists), do the following:

.. code-block::
  :caption: Troubleshooting Readoutshell (NSCL DAQ)

       cd ~
       startev
       cd /usr/opt/nscldaq/11.3-029/bin
       ./ringbuffer list # (This will list the active ringbuffers)
       ./ringbuffer delete full-name-of-whatever-ring-buffer(s)-you-want-to-delete

I am actually not sure if you should do this from the DAQ computer which is the host matchine building the buildring, or if you need to ssh to the :code:`spdaq57` or :code:`spdaq87` (whichever is complaining about an existing ringbuffer) and then delete that ringbuffer. In any case, try to ssh to the problematic :file:`spdaq` first, and follow the instructions given above to see if you have the permission to get to the :code:`/usr/opt/nscldaq/11.3-029/bin/` directory. If you do, then viola! If not, try this on the DAQ computer building the master ring without sshing to any of the :code:`spdaq` machines.

If these steps failed to solve the problem, try to reboot, the Pixie crates (both JENSA and focal plane 4 crates). You may need to reboot them more than once, and hopefully that will solve the problem.


nscope
------

nscope is a program developed in Root, with which one can communicate with the Pixie digitizers, see live spectra from various detectors, and set up the energy and timing filters, waveforms and traces, and all other required electronics setup for signal processing via the Pixie-16 digitizers.

To run nscope, make sure the DAQ is closed first and that there is no instance of Readout shell open anywhere. Then, from a computer that is connected to the DAQ network, run the following commands:

.. code-block::
  :caption: How to run nscope for JENSA detectors at the target location

       ssh -XY spdaq58
       startev
       cd ~/readout/crate_1
       nscope

.. code-block::
  :caption: How to run nscope for focal plane 4 detectors at the end of SECAR

       ssh -XY spdaq57
       startev
       cd ~/readout/crate_2
       nscope

Once nscope opens, one can change energy and timing filters, add or remove good channels, turn ON/OFF traces, change decay time, change the polarity of the signal, etc.

Scalers
-------

To set up the scalers for the first time, do the following:

.. code-block::
  :caption: How to set up Scalers for the first time

       cd ~
       git clone https://git.frib.msu.edu/secar/scalers.git
       startev
       cd ~/scalers
       python3 generate_scalers.py
       goscalers.sh

Make sure the scaler code is running before you run Readoutshell (DAQ). If you start the DAQ prior to the scalers, you will not be able to save the scalers for that run.

To run the scalers if they are already set up, do the following:

.. code-block::
  :caption: How to run Scalers

       goscalers.sh

If you change the DAQ version, the change has to be reflected on the scaler files as well. This is explained in the DAQ section above.

.. warning::

  Each time the scalers for a DAQ run are saved, the plots of a few selected scalers are also saved as .ps files. These files are saved under the :code:`scalers/` directory inside the home directory of the user account. The plotting files are usually quite large in the amount of disk space and may cause the home directory to be filled quickly. If that happens, the scalers program will quite and will give you an error saying it has run out of space to write files. At that point, you cannot run the scalers code anymore and will lose access to the scalers information for the future DAQ runs.

To fix the above mentioned issue, do the following:

.. code-block::
  :caption: How to stop Scalers to fill the home directory

     cd ~/scalers
     emacs generate_scalers.py
     # Comment out lines 67, 79, and 80 (lines starting with plot_scalers ...)
     # Save the changes
     startev
     python3 generate_scalers.py
     goscalers.sh
