# HUNT-JWT-CRYPTO: Forgeable JSON Web Tokens (A04 Cryptographic Failures)
token you forged; so you become another user or an admin without their secret.
- **`alg:none`**; the verifier trusts the token's own `alg` header. Set
- **RS256 → HS256 key confusion**; the token is signed RS256 (asymmetric). The
  HS256, it will use that public key as the HMAC *secret*; which you also know.
## Recon: is this app JWT-based?
## Forging the token (never hand-encode base64: use a JWT tool)
**alg:none; become admin / another user**
signature: (empty; keep the trailing dot:  header.payload. )
Some verifiers reject lowercase `none` but accept `None`/`NONE`/`nOnE`; try case variants.
**RS256 → HS256 key confusion; once you have the RSA public key**
**kid header injection; verifier loads the HMAC key from a FILE named by `kid`**
injection / SSRF if the key lookup hits a DB / shell / URL; same idea: `kid` is
**jku / x5u header injection (RS256); verifier fetches the public key from a URL in the token**
**jwk header self-signed key injection (RS256); embed an attacker-controlled public key in the token**
Remove "exp" (expiration) claim entirely; many validators skip the check if absent.
**Cross-tenant claim injection; escalate to another tenant's data via claim swaps**
Match the `payload` shape to a REAL token from the app (decode one first); keep
## Offline attacks: weak HMAC secret cracking
- `alg:none` rejected (401) just means that flaw is patched; try key confusion
- You need the recon, exploitation, or validation workflow described below; executed strictly inside the approved scope.