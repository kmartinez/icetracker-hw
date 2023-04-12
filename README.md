# GDP Glacier Monitor

## Name
GDP Glacier Monitor - Hardware - part of the glacsweb.org project at The University of Southampton

## File Info
```
├── GlacTracker_PSU_V1
│   ├── GlacTracker_PSU.brd - EAGLE board file
│   ├── GlacTracker_PSU.sch - EAGLE schematic file
│   ├── GlacTracker_PSU_BOM.xlsx - Overall BOM
│   ├── mgc1g18_GlacTracker_PSU.zip - Gerber Files
│   ├── mgc1g18_GlacTracker_PSU_JLC_bom.xls - BOM for JLC pick and place
│   ├── mgc1g18_GlacTracker_PSU_top_cpl.csv - CPL for JLC pick and place
├── GlacTrackerMCU_V1 
│   ├── GlacTracker.brd - EAGLE board file
│   ├── GlacTracker.sch - EAGLE schematic file
│   ├── mgc1g18_GlacTrackerMCU.zip - Gerber Files
├── GlacTrackerMCU_V2
│   ├── GlacTracker.brd - EAGLE board file
│   ├── GlacTracker.sch - EAGLE schematic file
├── GlacTracker7 - an Eagle7 respin of the separate PSU and MCU boards into a single board
│   ├── tracker.brd - EAGLE board file
│   ├── tracker.sch - EAGLE schematic file
```

### GlacTracker7
TBD

## GlacTracker_PSU
GlacTracker_PSU designed first to provide 3 configurable supplies through the MP2155 switching regulators. As well as a connection to the "battery" (the output of the BQ24074 Li-Ion battery Charger)
* The default settings are:
    * 3.3V
    * VCC_1 (set to 3.3V) with an optional 3.3V LDO which can be enabled by setting VCC_1 to around 4V (by changing resistor values) and connecting jumpers.
    * 5V
    * V_GSM - connection to the "battery" as described above

Each of these supplies can be enabled or disabled through both on board jumpers and via an external 6-way JST connector meaning when all supplies are disabled the leakage current is very low.

After testing it was found that using the V_GSM connector to power the GSM module was not viable as it cannot operate on voltages below 3.3V. Given this system uses Li-Ion batteries that can go as low as 2.8V, this was not a viable option.

Instead VCC_1 was set to 4V by changing R8 to 120k and two 470uF capacitors were added to the V_BAT input on the GSM module to handle current spikes during transmission. This means GSM can be used in all battery charge states.

* The current supply is setup as follows:
    * 3.3V - used to power MCU, GPS and radio. Enabled via RTC alarm enable circuit
    * VCC_1 - set to 4V used to power GSM. Enabled by MCU on demand
    * 5V - used to power SWARM. Enabled by MCU on demand
    * V_GSM - NO LONGER USED TO POWER GSM, when enabled can be used to read V_BAT

### Changes for V2
* V_GSM is now redundant as is being powered through 4V supply
* LDO is not being used to can be removed


# GlacTrackerMCU
This is the the "MCU" board with the MCU, GPS, Radio, GSM & SWARM.

The hardware is the same for a base station and rover, however the GSM & SWARM modules are not fitted on the rovers.


### GlacTrackerMCU_V2
Contains minor changes from V1 that were discovered after V1 had been "ordered":
* Capacitors added to V_BAT pin of GSM
* Diode removed from PSU_EN as not required.
