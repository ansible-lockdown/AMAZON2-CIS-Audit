# Changes to AMAZON2-CIS-Audit

August 2026
Based on CIS 4.0.0

- benchmark_version updated to v4.0.0, run_audit.sh BENCHMARK_VER updated to v4.0.0
- Restructured to the v4.0.0 seven section layout, mirroring the remediation role task grouping
  - section_4 added for the host based firewall controls, section_7 added for system maintenance
  - goss files regrouped under section_N/cis_<group>/ to match the remediation cis_<group>.x.yml files
- 37 retired controls removed, including all iptables, ip6tables and nftables tests
- 35 new controls added, covering the 1.5.x kernel hardening sysctls, the 3.3.1.x and 3.3.2.x
  per-sysctl split, the firewalld controls 4.1.4 to 4.1.8, 5.5.1.2, 5.5.2.2, 5.5.2.3, 5.5.2.8,
  6.1.1.7, 6.2.3.20 and 6.3.3
- vars/CIS.yml rule toggles resynced to 287, matching the remediation defaults and bridge template
- Titles resynced to the v4.0.0 benchmark
- v4.0.0 declares Level 1 - Server and Level 2 - Server only, so goss meta is now server-only
  (workstation: NA) and every level gate matches its control
- goss.yml rewritten for the new layout; the previous gate file referenced directories that
  no longer exist
- Renamed "cis_1.1.2.5.4 .yml" - the stray space in the filename hid it from content checks
- section_2/cis_2.1/cis_2.1.3.yml: asserted the literal `OPTIONS="-u chrony"`, but AL2 ships
  `OPTIONS="-F 2 -u chrony"` - a compliant host failed. Now a regex
- section_6/cis_6.3/cis_6.3.3.yml: regexes were double-escaped and wrapped in `/.../` while
  containing unescaped `/` in the `/sbin/...` paths, so they could never match. Now plain
  substring matches
- section_2/cis_2.1/cis_2.1.3.yml: restored the missing `file:` resource type - the block
  parsed as an unknown top-level resource so the control had never asserted anything
- section_4/cis_4.1/cis_4.1.3.yml: rewritten - doubled `---` marker, no resource type,
  unbalanced quote in the exec string, and it read firewall-cmd's `no zone` from stdout when
  firewalld writes it to stderr; now keyed off exit status
- section_4/cis_4.1/cis_4.1.7.yml: rewritten - doubled `---` marker, no resource type, and it
  asserted `!/Warning/` against a command that always echoes "Warning", so it could never pass
- vars/CIS.yml: added amazon2cis_active_firewall_zone and amazon2cis_pass_min_days, referenced
  by the new 4.1.4/4.1.6 and 5.5.1.2 tests but previously undefined
- renamed "cis_1.1.2.5.4 .yml" - the stray space hid it from content checks
- README updates and updated contributing and contributors

Based on CIS 3.0.0

- Initial release against the CIS Amazon Linux 2 Benchmark v3.0.0
