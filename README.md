# Amazon Linux 2 Goss config

## Overview

based on CIS 2.0.0 - 28th July 2021

This is bespoke to Amazon due to control IDs and associations. Redhat baselines can be more upto date but IDs and controls are not officially Amazon.

Set of configuration files and directories to run the stages of CIS of Amazon Linux 2 servers

This is configured in a directory structure level.

Goss is run based on the goss.yml file in the top level directory. This specifies the configuration.

## Join us

On our [Discord Server](https://discord.gg/JFxpSgPFEJ) to ask questions, discuss features, or just chat with other Ansible-Lockdown users

## Requirements

You must have [goss](https://github.com/aelsabbahy/goss/) available to your host you would like to test.

You must have sudo/root access to the system as some commands require privilege information.

Assuming you have already clone this repository you can run goss from where you wish.

Please refer to the audit documentation for usage.

- [Audit Documents](https://github.com/ansible-lockdown/AMAZON2-CIS-Audit/docs/Security_remediation_and_auditing.md)

This also works alongside the [Ansible Lockdown AMAZON2-CIS role](https://github.com/ansible-lockdown/AMAZON2-CIS)

Which will:

- install
- audit
- remediate
- audit

## variables

these are found in vars/cis.yml
Please refer to the file for all options and their meanings

CIS listed variable for every control/benchmark can be turned on/off or section

- other controls
enable_selinux
run_heavy_tasks

- bespoke options
If a site has specific options e.g. password complexity these can also be set.

## Usage

You must have [goss](https://github.com/aelsabbahy/goss/) available to your host you would like to test.

You must have root access to the system as some commands require privilege information.

- Run as root not sudo due to sudo and shared memory access

Assuming you have already clone this repository you can run goss from where you wish.

- full check

```sh
# {{path to your goss binary}} --vars {{ path to the vars file }} -g {{path to your clone of this repo }}/goss.yml --validate

```

example:

```sh
# /usr/local/bin/goss --vars ../vars/cis.yml -g /home/bolly/rh7_cis_goss/goss.yml validate
......FF....FF................FF...F..FF.............F........................FSSSS.............FS.F.F.F.F.........FFFFF....

Failures/Skipped:

Title: 1.6.1 Ensure core dumps are restricted (Automated)_sysctl
Command: suid_dumpable_2: exit-status:
Expected
    <int>: 1
to equal
    <int>: 0
Command: suid_dumpable_2: stdout: patterns not found: [fs.suid_dumpable = 0]


Title: 1.4.2 Ensure filesystem integrity is regularly checked (Automated)
Service: aidecheck: enabled:
Expected
    <bool>: false
to equal
    <bool>: true
Service: aidecheck: running:
Expected
    <bool>: false
to equal
    <bool>: true

< ---------cut ------- >

Title: 1.1.22 Ensure sticky bit is set on all world-writable directories
Command: version: exit-status:
Expected
    <int>: 0
to equal
    <int>: 123

Total Duration: 5.102s
Count: 124, Failed: 21, Skipped: 5

```

- running a particular section of tests

```sh
# /usr/local/bin/goss -g /home/bolly/rh7_cis_goss/section_1/cis_1.1/cis_1.1.22.yml  validate
............

Total Duration: 0.033s
Count: 12, Failed: 0, Skipped: 0

```

- changing the output

```sh
# /usr/local/bin/goss -g /home/bolly/rh7_cis_goss/section_1/cis_1.1/cis_1.1.22.yml  validate -f documentation
Title: 1.1.20 Check for removeable media nodev
Command: floppy_nodev: exit-status: matches expectation: [0]
Command: floppy_nodev: stdout: matches expectation: [OK]
< -------cut ------- >
Title: 1.1.20 Check for removeable media noexec
Command: floppy_noexec: exit-status: matches expectation: [0]
Command: floppy_noexec: stdout: matches expectation: [OK]


Total Duration: 0.022s
Count: 12, Failed: 0, Skipped: 0
```

## Variables

### The variable files

sections_2/service.yml allows you to tune it further for specific environments.

In this case installed or skipped using the standard name for a package to be installed or _skip to skip a test.

## Extra settings

Some sections can have several options in that case the skip flag maybe passed to the test.
e.g.

- section_1/cis/1.3.2 - aide_scan_type - can be cron or timer depending on site policy
- section_1/cis/1.8 - need to review the MOTD and issue files for bespoke content
- section_1/cis_1.10/cis_1.10.yml  - Has gdm either not installed or configured default to not installed and skipped configured.
- section_2/cis_2.2/cis_chrony_2.2.1.1.yml - this is chosen between ntp of chrony in the goss file

## further information

- [goss documentation](https://github.com/aelsabbahy/goss/blob/master/docs/manual.md#patterns)
- [CIS standards](https://www.cisecurity.org)
