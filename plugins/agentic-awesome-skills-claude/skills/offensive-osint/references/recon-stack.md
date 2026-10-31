| crt.sh | Free | Best single source for cert-derived subdomains; **frequently 502s during peak hours; see fallback chain below**. |
crt.sh runs on a single nginx in front of a busy Postgres; 502 / 503 / timeout in peak hours is routine. Don't retry-loop; pivot:
# 1. Censys cert search (free 250 queries/month with key): same data, different infra
# 2. Cert Spotter API (sslmate): free w/ rate limits
# 3. CertStream archive (Calidog): historical CT log mirror
# 4. Subfinder bundled aggregator (uses 30+ sources internally: Chaos, Anubis, BinaryEdge, BufferOver, Censys, CertSpotter, Crobat, Crtsh, DNSDumpster, FOFA, Fullhunt, GitHub, HackerTarget, IntelX, PassiveTotal, Quake, Rapiddns, Shodan, Spyse, ThreatBook, ThreatMiner, URLScan, VirusTotal, WhoisXML, ZoomEye, etc.)
# 5. AlienVault OTX: free, no key
# 6. ThreatMiner: free
# 7. URLScan: passive DNS via past scans
  "crt.sh down; pivot to Subfinder: subfinder -d $D -all -silent" | Out-Host
- [Shodan](https://www.shodan.io/), [Censys](https://search.censys.io/); internet device + cert search.
- [GreyNoise](https://viz.greynoise.io/); distinguish background noise from targeted scans.
- [SecurityTrails](https://securitytrails.com/); passive DNS + asset discovery.
- [SpiderFoot](https://www.spiderfoot.net/); automated recon + correlation.
- [theHarvester](https://github.com/laramies/theHarvester); subdomain, email, metadata.
- [Recon-ng](https://github.com/lanmaster53/recon-ng); web recon framework.
- [Amass](https://github.com/owasp-amass/amass) / [Subfinder](https://github.com/projectdiscovery/subfinder); passive subdomain.
- [BuiltWith](https://builtwith.com/); tech stack enumeration.
- [Netlas](https://netlas.io/); large-scale HTTP/DNS/cert pivots.
- [BinaryEdge](https://www.binaryedge.io/) / [FOFA](https://fofa.so/) / [ZoomEye](https://www.zoomeye.org/); Shodan/Censys complements.
- [RiskIQ PassiveTotal](https://community.riskiq.com/); passive DNS/cert/host pivots.
- [Spur](https://spur.us/); IP lookups.
- [Robtex](https://www.robtex.com/); passive DNS + infrastructure.
- `bgp.he.net` has no public API; HTML scraping only; fragile.
- **httpx (ProjectDiscovery)**; Wappalyzer-compatible ~600 signatures, JARM, favicon mmh3, TLS cert SHA256, security headers, screenshots. Use only the explicit approved host list and request budget; do not default to thousands of hosts.
- **JARM**; TLS handshake hash; stable per server config; useful for clustering.
- [ja3er.com](https://ja3er.com); community-curated JA3 → client-software mapping.
- [TLS Fingerprint DB](https://tlsfingerprint.io/); research aggregator.
- **RouteViews**; `http://archive.routeviews.org/` (free; historical BGP routing table snapshots).
- **RIPE RIS**; `https://ris.ripe.net/` (free; route collectors).