# HUNT-SPRINGBOOT: Spring Boot Specific Vulnerabilities
- **`/actuator/heapdump`**; full JVM heap dump contains plaintext passwords, tokens, DB credentials, private keys stored anywhere in memory
- **`/actuator/env`**; lists all environment variables and Spring properties including secrets
- **`/actuator/shutdown`**; POST → shuts down the application (Critical availability impact)
- **H2 Console (`/h2-console`)**; in-memory DB admin UI → SQL query execution → potential RCE via `CREATE ALIAS` trick
- **SpEL injection**; Spring Expression Language in template fields, `@Value` annotations, SpEL-processed request params → RCE
- **Spring4Shell CVE-2022-22965**; Spring Framework < 5.3.18 + Tomcat → RCE via data binding
## Phase 1: Fingerprint Spring Boot
## Phase 2: Actuator Endpoint Enumeration
  # Don't trust HTTP 200 alone: Spring returns 200 with a Whitelabel/login
## Phase 3: Heap Dump Analysis
# Download heap dump (can be large: 100MB+)
# Quick grep for secrets in heap dump (binary file: use strings)
## Phase 4: H2 Console RCE
## Phase 5: SpEL Injection
# RCE payload: note: exec() returns a Process, not a String, so a bare
# CVE-2022-22963: Spring Cloud Function SpEL
## Phase 5b: Spring Cloud Gateway Actuator SpEL RCE (CVE-2022-22947)
## Phase 5c: Writable `/env` -> `/refresh` gadget RCE
## Phase 6: Spring4Shell (CVE-2022-22965)
## Phase 7: Jolokia JMX Exposure
- You need the recon, exploitation, or validation workflow described below; executed strictly inside the approved scope.