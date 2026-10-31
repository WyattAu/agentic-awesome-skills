Reveals when volume changed; look for sudden spikes or drops.
- **Bound query cost and disclosure**; LIMIT bounds returned rows, not bytes scanned or sort/aggregation cost. Use partition filters, query estimates, timeouts and read-only credentials. Select necessary columns instead of sensitive SELECT * samples.
- **Use time filters**; always scope to the relevant time window around the incident.
- **Start broad, then narrow**; sample rows first, then targeted aggregations.
- **Compare before vs after**; the most powerful investigation pattern.
- **Follow the data upstream**; if this table looks wrong, check the source it reads from.