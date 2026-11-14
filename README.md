# pam-passkey.github.io

The approval and registration page of [pam-passkey](https://github.com/kawaz/pam-passkey), served by GitHub Pages at `https://pam-passkey.github.io/`.

`index.html` is the whole page: no external scripts, styles or fonts. It reads its fragment (`https://pam-passkey.github.io/#r=<presigned GET, percent-encoded>&k=<key, base64url>`, or `#touch-id:r=…&k=…` when pam-passkey opens it on the Mac), fetches the envelope at `r`, decrypts it with `k`, and shows an approval (`kind: approve`) or a registration (`kind: register`). If the key is missing or the envelope cannot be decrypted, it shows nothing of the request and does not sign. The formats are documented in pam-passkey's `docs/design/exchange-format.md`.

The Content-Security-Policy in `<meta>` allows the inline script and style by their SHA-256. After editing either, recompute both hashes and replace them in the policy:

```sh
python3 -c 'import base64,hashlib,re,sys; s=open("index.html").read(); [print(t, base64.b64encode(hashlib.sha256(re.search(f"<{t}>(.*?)</{t}>", s, re.S).group(1).encode()).digest()).decode()) for t in ("script","style")]'
```

Forks that host the page themselves set pam-passkey's `page_url` to their own origin; passkeys are bound to the page's host (the RP ID).
