# Log4Shell (CVE-2021-44228) Security Notes

## Summary

Log4Shell is a critical remote code execution vulnerability in Apache Log4j 2, exploitable via JNDI lookups in log messages. An attacker who can control log messages or message parameters can execute arbitrary code loaded from an LDAP (or other JNDI) server.

## CVE Identifier

- **CVE:** CVE-2021-44228

## Severity

- **CVSS 3.x Base Score:** 10.0 CRITICAL
- **Vector:** CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:C/C:H/I:H/A:H
- Rated as the most severe CVSS score ever recorded (10.0) by NVD.

## Affected Versions

- **Affected:** Apache Log4j2 **2.0-beta9 through 2.14.1** (log4j-core only; applications using only log4j-api without log4j-core are not impacted)
- **Fixed:** **2.15.0** (Java 8+), **2.12.2** (Java 7), **2.3.1** (Java 6)
- Note: The 2.15.0 fix was incomplete in certain non-default configurations (CVE-2021-45046). For complete remediation use **2.16.0** (Java 8+), **2.12.3** (Java 7), or **2.3.1** (Java 6), which also completely removes JNDI lookup functionality.

## Core Facts for Briefing

- Vendor: Apache Software Foundation; Product: Apache Log4j2
- Component affected: log4j-core JAR
- Only log4j-core is impacted; log4net, log4cxx, and other Apache Logging Services projects are not affected
- Vulnerability is specific to log4j-core and does not affect log4net, log4cxx, or other Apache Logging Services projects
- Discovered by Chen Zhaojun of Alibaba Cloud Security Team

## Remediation Actions

1. **Upgrade path:** Upgrade log4j-core to 2.15.0+ (Java 8+), 2.12.2 (Java 7), or 2.3.1 (Java 6); prefer 2.16.0+ to fully remove JNDI lookups.
2. **Detecting vulnerable deployments:** Audit dependencies and runtime for log4j-core versions in the 2.0-beta9–2.14.1 range; search logs for JNDI lookup patterns (e.g., ${jndi:ldap://...}).
3. **Hardening:** Disable JNDI lookups — set the system property `log4j2.formatMsgNoLookups` to `true` (Log4j 2.10–2.15), or remove the `JndiLookup` class from the classpath; set environment variable `LOG4J_FORMAT_MSG_NO_LOOKUPS=true`.
4. **Automated dependency scanning:** Enable Dependabot security updates, OWASP Dependency-Check, Snyk, or GitHubjects in CI to detect vulnerable Log4j versions in build graphs.

## References

- NVD — CVE-2021-44228: https://nvd.nist.gov/vuln/detail/cve-2021-44228
- Apache Logging Services Security Page: https://logging.apache.org/security.html
- CISA Alert (Dec 10, 2021): https://www.cisa.gov/news-events/alerts/2021/12/10/apache-releases-log4j-version-2150-address-critical-rce-vulnerability-under-exploitation
- Apache Log4j 2 Releases Page: https://logging.apache.org/log4j/2.x/