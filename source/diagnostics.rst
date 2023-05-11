 
Diagnostics
=========== 

This chapter presents a few instructions on how to operate SECAR diagnostics.

The CS-Studio page for SECAR diagnostics is found under SECAR Global Controls, "Diag. by Type" tab.

Viewers
-------

To move the viewers in and out, you may need to click on "Ignore" or "Override" check buttons before being able to move the viewers.

To turn ON/OFF the cameras, click on the associated "Settings" button. Then, click on the "Start" button next to "Acquire". The light can be turned ON/OFF using another process variable that has "LT" in it, for example: :code:`SCR_BTS34:LT_D1457`.

JENSA Viewer
~~~~~~~~~~~~

The JNESA viewer at :code:`SCR_BTS35:VD_D1457` is a new viewer upstream of the extended gas target. The indicator for viewer IN may still be ON even if the viewer is taken out. To fix this, please ask Dan Crisp or Francisco Pereira de Figueiredo to correct it in CS-Studio. 

Faraday Cups
------------

:code:`ReA_BTS34:FC_D1448` is controlled by ReA operators.

:code:`SCR_BTS35:FC_D1485` and :code:`SCR_BTS35:FC_D1568` are pnuematic. All other SECAR Faraday cups are controlled by motorized drives. The actual positons (in mm) of the cups can be seen from the "Drives" section of the "Diag. by Type" page. The Faraday cups can be biased by clicking on "Settings" buttons found under cups section of the Diag. by Type page. Please enter :math:`-200` V in "Bias Voltage SP" field and then turn ON the bias power supply ("Bias PS") and "START" the acquisition of the cup.

Slits
-----

SECAR has 5 slits, two of which are in focal plane 1. The other 3 are at each remaining focal plane. The slits at focal plane 1 are two separate slit systems, while the other 3 slits are assembled in one system each.

The slits are all operated via motorized drives seen in the "Slits" section of the Diag. by Type page. You can either move each slit (beam left and beam right) separately using "Left Setpoint" and "Right Setpoint", or you can move them together using "Center Setpoint" and "Gap Setpoint". 

.. note::

   The left slit in focal plane 4 has hysteresis. If you move it from one location to another, there will be a high chance that its actual position will differ from what its position readback indicates. To avoid this, fully retract it each time it needs to move into a new position first and then move it to the new position. Only then, the new position's readback accurately indicates its actual position.
   The position commands of all SECAR slits are accurate to within :math:`200 - 500 {\mu}m`.

Collimators
-----------

There are two collimators monted on a separate motorized drive: :code:`SCR_BTS35:DD_D1457` and :code:`SCR_BTS35:DD_D1568`. The former has 4 circular apertures with diameters of 1.5 mm, 2 mm, 4 mm, and 8 mm. The latter only has one aperture with diameter of 5 mm. These apertures can be put in by clicking on the appropriate button corresponding to each aperture. Each of these apertures can be retracted fully by clicking on the "Retract Drive" buttons.

JENSA Collimator
~~~~~~~~~~~~~~~~

This is currently packed in a box, which is with the vacuum group. Once you need to install the jet target chamber, you would need to also install this aperture.

MCP Foil and Mask Drives
------------------------

The upstream and downstream MCP detectors' foil and mask can be controlled by :code:`SCR_BTS35:DD_D1857` and :code:`SCR_BTS35:DD_D1871`, respectively. The "Foil in" button puts the foil in and the "Mask in" button puts the mask in. To retract the drive, click on "Retract foil" button.
