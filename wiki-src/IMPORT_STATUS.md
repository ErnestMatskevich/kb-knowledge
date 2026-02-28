# Wiki import status

Attempted to clone the wiki repository:

- `https://github.com/ErnestMatskevich/kb-knowledge.wiki.git`

Result in this environment:

- `CONNECT tunnel failed, response 403`

Because outbound access to GitHub is blocked in this runtime, `.md` files from the wiki could not be fetched automatically.

To complete the import in an environment with GitHub access, run:

```bash
git clone https://github.com/ErnestMatskevich/kb-knowledge.wiki.git /tmp/kb-knowledge.wiki
mkdir -p wiki-src
find /tmp/kb-knowledge.wiki -type f -name '*.md' -exec cp --parents {} wiki-src/ \;
```
