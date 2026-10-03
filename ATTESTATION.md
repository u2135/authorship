# u2135 authorship

Public key and signature for a **private claim file**. The claim text is **not** in this repository.

## Files

| File | Purpose |
|------|---------|
| `u2135-authorship.pub` | SSH public key |
| `claim.txt.sig` | OpenSSH signature over the claim |
| `claim.sha256` | SHA-256 (hex) of the exact claim bytes that were signed |

## Verification

The author provides `claim.txt` privately. Its hash must match `claim.sha256`.

```bash
curl -fsSL -o u2135-authorship.pub \
  https://raw.githubusercontent.com/u2135/authorship/main/u2135-authorship.pub
curl -fsSL -o claim.txt.sig \
  https://raw.githubusercontent.com/u2135/authorship/main/claim.txt.sig
curl -fsSL -o claim.sha256 \
  https://raw.githubusercontent.com/u2135/authorship/main/claim.sha256

sha256sum claim.txt | awk '{print $1}' | diff - claim.sha256

ssh-keygen -Y verify -f u2135-authorship.pub -I u2135-authorship -n file \
  -s claim.txt.sig < claim.txt
```

A good signature means the claim was signed by the private key matching `u2135-authorship.pub`.
