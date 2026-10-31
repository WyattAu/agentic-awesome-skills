This means: every public behavior; including undocumented quirks, error message text, timing, and ordering; becomes a de facto contract once users depend on it. Design implications:
Avoid forcing consumers to choose between multiple versions of the same dependency or API. Diamond dependency problems arise when different consumers need different versions of the same thing. Design for a world where only one version exists at a time; extend rather than fork.
Define the interface before implementing it. The contract is the spec; implementation follows.
  // Partial update; only provided fields change
  // Idempotent delete; succeeds even if already deleted
**Don't mix patterns.** If some endpoints throw, others return null, and others return `{ error }`; the consumer can't predict behavior.
  // description: string;  // Removed; breaks existing consumers
  priority: number;         // Changed from string; breaks existing consumers
Accept partial objects; only update what's provided: