---
description: PHC Modules
---

##Intro
The PHC (PEHA Home Control) domotic system knows a fair amount of different modules, 
each module:  
- is identified by a physical module type (phy)  
- communicates via a binary protocol  
- has a specific amount/type of channels (input/output/feedback)  
- belongs to a module class (input/multifunction/output/analog/box/dimmer)  
- has a settable address via DIP switches (0-31)  
- has a dedicated set of supported commands and generated responses

Within a module class there are 32 adddresses (0-31) available, each module needs to be mapped
to a unique address within it's class, otherwise conflicts will occur.
Note that some modules use multiple addresses to cover it's functionality.

Phc2Mqtt is the brigde between the module's binary protocol, and the textual interface with humans.
As such Phc2Mqtt needs to know the physical module types to perform a correct communication with each module.
On the human side it offers a textual command/response format that uses a logical module type (log).

Following shows the relation between module class and logical module type:

| Class | Logical Module Types
|-------|-----------
| Input modules | imd, imw, tab, et0
| Multifunction modules | utm, bwm, uim, et1, mcc
| Output modules | omd, omx, jrm
| Analog modules | amd, fui, fu1, fu2
| Box modules | ebs, ebr, ebd, mls
| Dimmer modules  | dim, dal

##Project Information
The information needed by Phc2Mqtt is all put together in the project information file (i.e. project.stm).

###Proxy Mode
When running Phc2Mqtt in Proxy mode it will passively listen on the PHC module bus to report activity
and send commands to the real STM that will handle protocol details for communication with the PHC modules.

In this mode Phc2Mqtt needs following information per module:  
- physical module type  
- module address setting

###PassiveSTMv3 Mode
When running Phc2Mqtt in PassiveSTMv3 mode the module list is required but not enough.

In this mode Phc2Mqtt takes over the role of the STM and it needs additional information, this requires some explanation:  
- The PHC module bus is slow (19200 baud) and shared by all modules  
- The binary protocol tries to reduce the number of packets sent over it to avoid contention  
- When a PHC module starts it will do only 1 thing: send a boot packet to it's master (STM)  
- The STM responds with a config packet, now the module knows which events to report to STM  
- After that the PHC module will report the enabled events to STM

The additional information for each input (in/ir/il/im) and feedback (fb) channel of the module is:  
- Channel identification  
- Optional a descriptive text for this channel  
- A list of all events enabled for this channel

Output channels (mrk/led/out) can optionally specify following info:  
- Channel identification  
- A description of this channel

##Project Information Syntax
Project info can span 1 or more lines but the last line always ends on a semicolon (;).

Each line in project info carries 1 functional block as shown in the Backus-Naur format below, the split over lines is intentional and strict.

A space character is used to separate fields and can thus not be used as data.

Keywords and delimiters (between double quotes) are case-insensitive, actual data strings are case-sensitive.

```
config         ::= "<stmd>"
                   *[<project> | <module> | <comment>]
                   "</stmd>"

comment        ::= ";" [<string>]

project        ::= "project" "name="<string> ["author="<string>] ";"

module         ::= "module" "id="<mod>.<address>
                     *("channel" <channel>) ";"

  mod          ::= physical module name
  address      ::= 0-31

channel        ::= "id="<id> "desc="<string> ["events="<event> *[","<event>]
  id           ::= <chn-type><chn-num>
  chn-type     ::= "in" | "ir" | "il" | "im" | "mrk" | "led" | "out" | "fb"
  chn-num      ::= 0-31
  event        ::= <string>

string         ::= *("a-z" | "A-Z" | "0-9" | "_-/.,"':;{}")
```

##Module Templates
Building the project info file from scratch is a difficult task, therefore we created templates for each physical module type.

Below table lists all modules, their physical and logical module types, their image and manual and the corresponding template file.

Copy the template for each module you have into your project.stm file and make adjustments:  
- adjust the <addr> of the module  
- put all not used channels in comment  
- optional adjust the desc field of each channel
- for the input/feedback channels:  
  - remove the non used events from the event-list  
  - for light control we typiclly retain: outlt1 and ingt2  
  - for shutter control we typically retain: ingt0 and ingt1


| Phy(Template) | Log | PHC product | Description/Module definition | Image(Manual)  
|-----|-----|-------------|-------------------------------|---------------
| [EMD24](phc/emd24.md) | imd | 940/24EM | Input module 24 V | [<img style="float:right;width:100px;height:100px" src="../phc/EMD24.png"></img>](../phc/EMD24.pdf)
| [EMD_RUE](phc/emd_rue.md) | imd | 940/24EM RU<br/>940/24EM RU diag    | Input module 24 V LED | [<img style="float:right;width:100px;height:100px" src="../phc/EMD_RUE.png"></img>](../phc/EMD_RUE.pdf)
| [EMD_24S](phc/emd_24s.md) | imd | 941/24 EM           | Input module 24 V switch | [<img style="float:right;width:100px;height:100px" src="../phc/EMD_24S.png"></img>](../phc/EMD_24S.pdf)
| [EMD230](phc/emd230.md) | imd | 940/230 EM          | Input module 230 V | [<img style="float:right;width:100px;height:100px" src="../phc/EMD230.png"></img>](../phc/EMD230.pdf)
| [UEM_1](phc/uem_1.md) | imw | 940/24 EM UP o.T.   | Flush mount input module 24 V DC | [<img style="float:right;width:100px;height:100px" src="../phc/UEM_1.png"></img>](../phc/UEM_1.pdf)
| [UEM_2](phc/uem_2.md) | imw | 940/24 EM RU o.T.   | Flush mount input module with LED | [<img style="float:right;width:100px;height:100px" src="../phc/UEM_2.png"></img>](../phc/UEM_2.pdf)
| [UEM_2](phc/uem_2.md) | imw | OpenHC UEM          | Flush mount input module with LED | [<img style="float:right;width:100px;height:100px" src="../phc/UEM.jpg"></img>](../phc/UEM.jpg)
| [ET0-2](phc/et0-2.md) | et0 | 940/2.xx ET(B)      | Flush mount push button 2 fold | [<img style="float:right;width:100px;height:100px" src="../phc/ET0-2.png"></img>](../phc/ET0-8.pdf)
| [ET0-4](phc/et0-4.md) | et0 | 940/4.xx ET(B)      | Flush mount push button 4 fold | [<img style="float:right;width:100px;height:100px" src="../phc/ET0-4.png"></img>](../phc/ET0-8.pdf)
| [ET0-8](phc/et0-8.md) | et0 | 940/8.xx ET(B)      | Flush mount push button 8 fold | [<img style="float:right;width:100px;height:100px" src="../phc/ET0-8.png"></img>](../phc/ET0-8.pdf)
| [ET0-D](phc/et0-d.md) | et0 | 940/8.xx ET D       | Flush mount push button 8 fold with display | [<img style="float:right;width:100px;height:100px" src="../phc/ET0-D.png"></img>](../phc/ET0-D.pdf)
| [EMD_TAB](phc/emd_tab.md)   | tab | 941/16 TAB          | Indicator- and control panel  (16 push buttons) | [<img style="float:right;width:100px;height:100px" src="../phc/EMD_TAB.png"></img>](../phc/EMD_TAB.pdf)
| [EMD_TAB_2](phc/emd_tab_2.md) | tab | 941/32 TAB          | Indicator- and control panel  (32 push buttons) | [<img style="float:right;width:100px;height:100px" src="../phc/EMD_TAB_2.png"></img>](../phc/EMD_TAB.pdf)
| [EMD_VIR](phc/emd_vir.md) | imd | -                   | Virtual input module | -
| [UTM_1.0](phc/utm_1.md) | utm | 941/1 UP o.A          | Push button, 1-fold | [<img style="float:right;width:100px;height:100px" src="../phc/UTM_1.png"></img>](../phc/uim.pdf)
| [UTM_2.1](phc/utm_2.md) | utm | 941/2 UP o.A          | Push button, 2-fold | [<img style="float:right;width:100px;height:100px" src="../phc/UTM_2.png"></img>](../phc/uim.pdf)
| [UTM_3.2](phc/utm_3.md) | utm | 941/4 UP o.A          | Push button, 4-fold | [<img style="float:right;width:100px;height:100px" src="../phc/UTM_3.png"></img>](../phc/uim.pdf)
| [MFM_BWM](phc/mfm_bwm.md) | bwm | 410 BM o.A     | Flush - mounting motion detector 180degr | [<img style="float:right;width:100px;height:100px" src="../phc/MFM_BWM.png"></img>](../phc/MFM_BWM.png)
| [UIM](phc/uim.md) | uim | 941 UP o.A          | Infrared module | [<img style="float:right;width:100px;height:100px" src="../phc/UIM.png"></img>](../phc/uim.pdf)
| [ET1-2](phc/et1-2.md) | et1 | 940/2.xx ET(B) | Flush mounting push button 2 fold | [<img style="float:right;width:100px;height:100px" src="../phc/ET1-2.png"></img>](../phc/ET0-8.pdf)
| [ET1-4](phc/et1-4.md) | et1 | 940/4.xx ET(B)      | Flush mounting push button 4 fold | [<img style="float:right;width:100px;height:100px" src="../phc/ET1-4.png"></img>](../phc/ET0-8.pdf)
| [ET1-8](phc/et1-8.md) | et1 | 940/8.xx ET(B)      | Flush mounting push button 8 fold | [<img style="float:right;width:100px;height:100px" src="../phc/ET1-8.png"></img>](../phc/ET0-8.pdf)
| [ET1-D](phc/et1-d.md) | et1 | 940/8.xx ET D       | Flush mounting push button 8 fold with display | [<img style="float:right;width:100px;height:100px" src="../phc/ET1-D.png"></img>](../phc/ET0-D.pdf)
| [MCC_2](phc/mcc_2.md) | mcc | 940 MCC             | Multi Control Center (MCC) | [<img style="float:right;width:100px;height:100px" src="../phc/MCC_2.png"></img>](../phc/MCC_2.pdf)
| [AMD230_4](phc/amd230_4.md) | omd | 940/4 AM<br/>940/4 AM diag     | Output module 230V/4A | [<img style="float:right;width:100px;height:100px" src="../phc/AMD230_4.png"></img>](../phc/AMD230_4.pdf)
| [AMD230_10](phc/amd230_10.md) | omd | 940/10 AM<br/>940/10 AM diag   | Output module 230V/10A |  [<img style="float:right;width:100px;height:100px" src="../phc/AMD230_10.png"></img>](../phc/AMD230_10.pdf)
| [AMD230_16](phc/amd230_16.md) | omd | 940/16 AM           | Output module 230V/16A |  [<img style="float:right;width:100px;height:100px" src="../phc/AMD230_16.png"></img>](../phc/AMD230_16.pdf)
| [ALP](phc/alp.md) | omd | 940/24 AM | Output module 24V | [<img style="float:right;width:100px;height:100px" src="../phc/alp.png"></img>](../phc/alp.pdf)
| [AMD_EVG](phc/amd_evg.md) | omd | 941/10 AM | Output module 230V/EVG | [<img style="float:right;width:100px;height:100px" src="../phc/AMD_EVG.png"></img>](../phc/AMD_EVG.pdf)
| [AMD230_4_man_ext](phc/AMD230_4_MAN_EXT.md)  | omx | 942/4 AM<br/>942/4 AM diag     | Output module 230V/4A diag |  [<img style="float:right;width:100px;height:100px" src="../phc/AMD230_4_MAN_EXT.png"></img>](../phc/AMD230_4_MAN_EXT.pdf)
| [AMD230_10_man_ext](phc/AMD230_10_MAN_EXT.md) | omx | 942/10 AM<br/>942/10 AM diag   | Output module 230V/10A diag | [<img style="float:right;width:100px;height:100px" src="../phc/AMD230_10_MAN_EXT.png"></img>](../phc/AMD230_10_MAN_EXT.pdf)
| [AMD230_16_man_ext](phc/AMD230_16_MAN_EXT.md) | omx | 940/16 AM MAN       | Output module 230V/16A manual | [<img style="float:right;width:100px;height:100px" src="../phc/AMD230_16_MAN_EXT.png"></img>](../phc/AMD230_16_MAN_EXT.pdf)
| [JRM24](phc/jrm24.md) | jrm | 940/24 JRM | Shutter/ blind module 24V | [<img style="float:right;width:100px;height:100px" src="../phc/JRM24.png"></img>](../phc/JRM24.pdf)
| [JRM](phc/jrm.md) | jrm | 940 JRM<br/> 940 JRM DIAG | Shutter/ blind module 230V | [<img style="float:right;width:100px;height:100px" src="../phc/JRM.png"></img>](../phc/JRM.pdf)
| [AMA(phc/ama.md)]() | amd | 940 AMA | Analogue module | [<img style="float:right;width:100px;height:100px" src="../phc/ama.png"></img>](../phc/ama.pdf)
| [MFM_FUNK](phc/mfm_funk.md) | fui | 940 FU C | RF- interface (Easyclick) | [<img style="float:right;width:100px;height:100px" src="../phc/MFM_FUNK.png"></img>](../phc/MFM_FUNK.pdf)
| [MFM_FUNK1](phc/mfm_funk1.md) | fu1 | 940 FU | RF- interface I/O (Easywave) | [<img style="float:right;width:100px;height:100px" src="../phc/MFM_FUNK1.png"></img>](../phc/MFM_FUNK1.pdf)
| [MFM_FUNK2](phc/mfm_funk2.md) | fu2 | 941 FU C | RF- interface I/O (Easyclick) | [<img style="float:right;width:100px;height:100px" src="../phc/MFM_FUNK2.png"></img>](../phc/MFM_FUNK2.pdf)
| [OBO_1](phc/obo_1.md) | ebs | 960/3 PSB | System box switching | [<img style="float:right;width:100px;height:100px" src="../phc/OBO_1.png"></img>](../phc/AMD_BUSBOX_S.pdf)
| [OBO_2](phc/obo_2.md) | ebr | 960/3 PSB/JR | System box shutter | [<img style="float:right;width:100px;height:100px" src="../phc/OBO_2.png"></img>](../phc/AMD_BUSBOX_R.pdf)
| [OBO_3](phc/obo_3.md) | ebd | 960/3 PSB/D | System box dimming (1-10V) | [<img style="float:right;width:100px;height:100px" src="../phc/OBO_3.png"></img>](../phc/OBO_3.pdf)
| [DIM_AN](phc/dim_an.md) | dim | 944/2 DIM AN | Forward phase control dimmer 420W | [<img style="float:right;width:100px;height:100px" src="../phc/DIM_AN.png"></img>](../phc/DIM_AN.pdf)
| [DIM_AB](phc/dim_ab.md) | dim | 944/2 DIM AB | Inverse phase control dimmer 420W | [<img style="float:right;width:100px;height:100px" src="../phc/DIM_AB.png"></img>](../phc/DIM_AB.pdf)
| [DIM_AN1000](phc/dim_an1000.md) | dim | 949 DM M-AN | Forward phase control dimmer 1000W | [<img style="float:right;width:100px;height:100px" src="../phc/DIM_AN1000.png"></img>](../phc/DIM_AN1000.pdf)
| [DIM_AB1000](phc/dim_ab1000.md) | dim | 949 DM M-AB | Inverse phase control dimmer 1000W | [<img style="float:right;width:100px;height:100px" src="../phc/DIM_AB1000.png"></img>](../phc/DIM_AB1000.pdf)
| [DIM_SAN](phc/dim_san.md) | dim | 944/2 DM AN<br/>949 DM M-AN | Synchronized forward phase control dimmer | [<img style="float:right;width:100px;height:100px" src="../phc/DIM_SAN.png"></img>](../phc/DIM_SAN.pdf)
| [DIM_SAB](phc/dim_sab.md) | dim | 944/2 DM AB<br/>949 DM M-AB | Synchronized inverse phase control dimmer | [<img style="float:right;width:100px;height:100px" src="../phc/DIM_SAB.png"></img>](../phc/DIM_SAB.pdf)
| [DIM_UNI](phc/dim_uni.md) | dim | 949 DM UN | Universal dimmer | [<img style="float:right;width:100px;height:100px" src="../phc/DIM_UNI.png"></img>](../phc/DIM_UNI.pdf)
| [DIM_DALI_8_EXT](phc/dim_dali_8_ext.md) | dal | 940/8 DALI-G | DALI-Gateway (8-channels) | [<img style="float:right;width:100px;height:100px" src="../phc/DIM_DALI_8_EXT.png"></img>](../phc/DIM_DALI_8.pdf)


The module and optional channel/event list can be provided in several ways:
- by uploading the project definition from the System Software v3 to Phc2Mqtt via the management interface
- by extracting project.ppfx from the project.zpfx file and then uploading it manual

In future we will obsolete both these methods and replace it with a full manual solution, this may sound strange but it will 
allow for a lot of functionality on the module to be removed and a significant reduction is required memory (64Kb Flash + 64Kb RAM).

To provide the manual configuration you will need to prepare a text file with below syntax and then upload it to Phc2Mqtt.
To avoid typing errors we will make use of prepared template files per module type that can be copy/pasted with minor adjustements.

For Proxy mode you need to copy the module definition, for PassiveSTMv3 mode you need to copy the module and channel definitions.

