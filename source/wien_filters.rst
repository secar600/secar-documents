 
Wien Filters
============ 

.. warning::
   Wien Filters can only be operated by specifically trained personnel following the `SECAR Wien Filter Conditioning and Operation Procedure <https://portal.frib.msu.edu/sites/dcc/pages/dcclink.aspx?WBS=M41600&Sub=PR&SN=001005>`_.

Wien Filter Users
~~~~~~~~~~~~~~~~~

.. warning::
    Only trained Wien Filter users can use the Wien Filter controls. Controls are blocked based on your login and only available for trained users registered in the channel control document <https://portal.frib.msu.edu/sites/dcc/pages/dcclink.aspx?WBS=M41600&Sub=RC&SN=006340>. As a trained user make sure you login with your credentials when operating the Wien filter, do not leave the computer you are logged into unattended, and log out once you are done. 

Some notes on operation: 

- At the beginning of your shift the lead operator must communicate the shift limit to you in terms of voltage, magnet settings and any survey thresholds. Make sure you know the limits and never exceed them. Do not change ramp and limits fields in the GUI. 
- WF1 has negative magnet current, WF2 has positive magnet current (need to have sign correct when entering values)
- Magnet Cycling: The magnet cycling procedure built into the control system is not working properly. Magnetic field should only be adjusted downwards. Cycle the magnets when you first use them, and each time you need to increase the field. Use the following procedure: 
    - Set magnetic field to zero and waith 3 min.
    - Increase current to 540 A, which is the maximum current. The control system takes care of the ramp rate. 
    - Once the current is at 540A, wait for 3 minutes
    - Then decrease the current to zero
    - Once the current is at zero, wait for 3 minutes
    - Increase the current again to 540 A. 
    - Once the current is at 540A, wait again for 3 minutes
    - Then decrease the current to the desired set point
- Interlocks
    - Wien Filter high voltage is interlocked with various interlocks. These include 
        - Count of >50% voltage drops
        - dV/dT exceeds the set limit more than five times
        - Vacuum limit (latch is displayed on regular Vacuum by Type CS-Studio page)
        - GV to Turbo pump (see regular Vacuum CS-Studio page)
        - There are also safety interlocks triggered by radiation monitors and a switch on the shielding doors for acccess to the cryo pumps. 
     - When interlocks trigger: 
        - For the software interlocks, click Reset on the WF controls GUI
        - For safety interlocks it may be necessary to go into the vault to the safety control panel
        - It may be necessary to switch the HV power supplies back to remote controllable mode. Check that the red buttion in the upper right corner is illiminated. If not, press the red button to switch it on. 



A few notes from Kiana:

- If you need to increase the allowed current on any electrode, make sure you first set the "High Current" to the desired :code:`I Setpoint + 10`. This will ensure that this "High Current" is not reached. In other words, it should always be 10 A higher than the set point for current. Otherwise, if "High Current" is smaller than the set point for current, it will interefer with how high the current is allowed to reach by the PLC.

- The :code:`DVDT` set point needs to be chosen wisely. The :code:`DVDT MAX` reading is the highest recorded value since the last "reset" (button below the :code:`DVDT MAX` reading) to help gauge where to set the number. If the PLC scan time is 10 ms (for example), and the voltage drops 5 kV between PLC scans, then :code:`DVDT` would be :math:`- 500000`. So, basically it counts any sudden drops in voltage as an arc. 5 of these arcs will turn the HV power supply OFF.

- Operation of the vacuum systems and vacuum interlocks of both Wien filters have changed. Please refer to the :numref:`Wien_filters_vacuum` for the updates.
