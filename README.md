# What is this repo for?

This repo is used as a custom BMC Pulse plugin. Currently the repository has to have public view rights in order for the plugin to be installable through the Pulse web GUI.
This Pulse plugin is built to be a way to collect Windows performance counters that are not built into the default collection agent supplied by Pulse.

### Prerequisites

PulsePlugin_Perfmon.exe requires that .NET Framework 4 be installed.

|     OS    | Linux | Windows | SmartOS | OS X |
|:----------|:-----:|:-------:|:-------:|:----:|
| Supported |       |    v    |         |      |

## Performance Counter Collection Setup

Which Windows performance counters are collected is determined by (1) the staticParams.json file and (2) the param.json file created upon install of the plugin.

(1) the staticParams.json file contains blocks of performance counter definitions. Each block has a keyword, such as 'API', 'COR', or 'WEB' and these keywords match up to the TAG property
that is set on each Pulse meter upon install. If you are unsure what tags a particular meter has, you can view them by opening the meter.conf file in the Pulse installation directory, then look
for the json property called "tags" under the "premium_api" object.  If a meter has the 'API' tag set, then the Windows performance counters defined in the 'API' block in the staticParams.json
file will be used.

(2) the param.json file. This file is created when you install this Pulse plugin to a particular meter. The param.json file will be blank by default, but can be populated with values if you
add counters during the plugin installation process in the Pulse web GUI.  These counters will be collected in addition to the counters defined in (1) staticParams.json file.

## How does this application run?

After this plugin has been installed to a Pulse meter, the PulsePlugin_Perfmon.exe file will be executed. This exe is developed to run in an infinite loop. It will start one thread for each
Windows performance counter to be collected. Each performance counter will be collected according to it's own polling interval (defined as 'defaultResolutionMS' in staticParams.json).