# Splunk Attack Range ⚔️

| branch | build status |
| ---    | ---          |
| develop| [![develop status](https://circleci.com/gh/splunk/attack_range/tree/develop.svg?style=svg&circle-token=67ad1fa7779c57d7e5bcfc42bd617baf607ec269)](https://circleci.com/gh/splunk/attack_range/tree/develop)|

## Purpose 🛡
The Attack Range is a detection development platform, which solves three main challenges in detection engineering. First, the user is able to build quickly a small lab infrastructure as close as possible to a production environment. Second, the Attack Range performs attack simulation using different engines such as Atomic Red Team or Caldera in order to generate real attack data. Third, it integrates seamlessly into any Continuous Integration / Continuous Delivery (CI/CD) pipeline to automate the detection rule testing process.  


## Demo 📺
[A short demo (< 6 min)](https://www.youtube.com/watch?v=xIbln7OQ-Ak) which shows the basic functions of the attack range. It builds a testing enviroment using terraform, walks through the data collected by Splunk. Then attacks it using MITRE ATT&CK Technique [T1003](https://attack.mitre.org/techniques/T1003/) and finally showcases how [ESCU](https://github.com/splunk/security-content) searches are used to detect the attack.

[![Attack Range Demo](https://img.youtube.com/vi/xIbln7OQ-Ak/0.jpg)](https://www.youtube.com/watch?v=xIbln7OQ-Ak)

## Building 👷‍♂️

Attack Range can be built in three different ways:

- **cloud** using terraform and AWS.
- **locally** with vagrant and virtualbox see the [attack\_range\_local](https://github.com/splunk/attack_range_local/) project for details
- **cloud-only** see the [attack\_range\_cloud](https://github.com/splunk/attack_range_cloud/) project for details 

## Architecture 🏯
![Logical Diagram](docs/attack_range_architecture1.png)

The virtualized deployment of Attack Range consists of:

- Windows Domain Controller
- Windows Server
- Windows Workstation
- A Kali Machine
- Splunk Server
- Phantom Server

Which can be added/removed/configured using [attack_range.conf](attack_range.conf). More machines such as Phantom, Linux server, Linux client, MacOS clients are currently under development.

An approxiamte **cost estimate** for running attack_range on AWS can be found [here](https://github.com/splunk/attack_range/wiki/Cost-Estimates---mode-terraform).

#### Logging
The following log sources are collected from the machines:

- Windows Event Logs (```index = win```)
- Sysmon Logs (```index = win```)
- Powershell Logs (```index = win```)
- Network Logs with Splunk Stream (```index = main```)
- Attack Simulation Logs from Atomic Red Team and Caldera (```index = attack```)


## Configuration 🏗

### [For Ubuntu 18.04](https://github.com/splunk/attack_range/wiki/Ubuntu-18.04-Terraform-Installation)

### [For MacOS](https://github.com/splunk/attack_range/wiki/Configure-Attack-Range-for-Terraform)

## Running 🏃‍♀️
Attack Range supports different actions:

- Build Attack Range
- Perform Attack Simulation
- Test with Attack Range
- Destroy Attack Range
- Stop Attack Range
- Resume Attack Range
- Dump Log Data from Attack Range

### Build Attack Range
- Build Attack Range
```
python attack_range.py -a build
```

### Perform Attack Simulation
- Perform Attack Simulation
```
python attack_range.py -a simulate -st T1003.001 -t attack-range-windows-domain-controller
```

### Test with Attack Range
- Automated testing of detection:
```
python attack_range.py -a test -tf tests/T1003_001.yml
```

### Destroy Attack Range
- Destroy Attack Range
```
python attack_range.py -a destroy
```

### Stop Attack Range
- Stop Attack Range
```
python attack_range.py -a stop
```

### Resume Attack Range
- Resume Attack Range
```
python attack_range.py -a resume
```

### Dump Log Data from Attack Range
- Dump Log Data from Attack Range
```
python attack_range.py -a dump -dn data_dump
```


## Features 💍
- [Splunk Server](https://github.com/splunk/attack_range/wiki/Splunk-Server)
  * Indexing of Microsoft Event Logs, PowerShell Logs, Sysmon Logs, DNS Logs, ...
  * Preconfigured with multiple TAs for field extractions
  * Out of the box Splunk detections with Enterprise Security Content Update ([ESCU](https://splunkbase.splunk.com/app/3449/)) App
  * Preinstalled Machine Learning Toolkit ([MLTK](https://splunkbase.splunk.com/app/2890/))
  * Splunk UI available through port 8000 with user admin
  * ssh connection over configured ssh key

- [Splunk Enterprise Security](https://splunkbase.splunk.com/app/263/)
  * [Splunk Enterprise Security](https://splunkbase.splunk.com/app/263/) is a premium security solution requiring a paid license.
  * Enable or disable [Splunk Enterprise Security](https://splunkbase.splunk.com/app/263/) in [attack_range.conf](attack_range.conf)
  * Purchase a license, download it and store it in the apps folder to use it.

- [Splunk Phantom](https://www.splunk.com/en_us/software/splunk-security-orchestration-and-automation.html)
  * [Splunk Phantom](https://www.splunk.com/en_us/software/splunk-security-orchestration-and-automation.html) is a Security Orchestration and Automation platform
  * For a free development license (100 actions per day) register [here](https://my.phantom.us/login/?next=/)
  * Enable or disable [Splunk Phantom](https://www.splunk.com/en_us/software/splunk-security-orchestration-and-automation.html) in [attack_range.conf](attack_range.conf)

- [Windows Domain Controller & Window Server & Windows 10 Client](https://github.com/splunk/attack_range/wiki/Windows-Infrastructure)
  * Can be enabled, disabled and configured over [attack_range.conf](attack_range.conf)
  * Collecting of Microsoft Event Logs, PowerShell Logs, Sysmon Logs, DNS Logs, ...
  * Sysmon log collection with customizable Sysmon configuration
  * RDP connection over port 3389 with user Administrator

- [Atomic Red Team](https://github.com/redcanaryco/atomic-red-team)
  * Attack Simulation with [Atomic Red Team](https://github.com/redcanaryco/atomic-red-team)
  * Will be automatically installed on target during first execution of simulate
  * Atomic Red Team already uses the new Mitre sub-techniques

- [Caldera](https://github.com/mitre/caldera)
  * Adversary Emulation with [Caldera](https://github.com/mitre/caldera)
  * Installed on the Splunk Server and available over port 8888 with user admin
  * Preinstalled Caldera agents on windows machines

- [Kali Linux](https://www.kali.org/)
  * Preconfigured Kali Linux machine for penetration testing
  * ssh connection over configured ssh key


## Support 📞
Please use the [GitHub issue tracker](https://github.com/splunk/attack_range/issues) to submit bugs or request features.

If you have questions or need support, you can:

* Post a question to [Splunk Answers](http://answers.splunk.com)
* Join the [#security-research](https://splunk-usergroups.slack.com/messages/C1RH09ERM/) room in the [Splunk Slack channel](http://splunk-usergroups.slack.com)
* If you are a Splunk Enterprise customer with a valid support entitlement contract and have a Splunk-related question, you can also open a support case on the https://www.splunk.com/ support portal

## Contributing 🥰
We welcome feedback and contributions from the community! Please see our [contribution guidelines](docs/CONTRIBUTING.md) for more information on how to get involved.

## Author
* [Jose Hernandez](https://twitter.com/d1vious)
* [Patrick Bareiß](https://twitter.com/bareiss_patrick)

## Contributors
* [Bhavin Patel](https://twitter.com/hackpsy)
* [Rod Soto](https://twitter.com/rodsoto)
* Russ Nolen
* Phil Royer
* [Joseph Zadeh](https://twitter.com/JosephZadeh)
* Rico Valdez
* [Dimitris Lambrou](https://twitter.com/etz69)
* [Dave Herrald](https://twitter.com/daveherrald)
