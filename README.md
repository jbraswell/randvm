## randomvm

to get started run

```bash
npm install
npm run dev
```

### github pages deploy

to build run
`npm run build`

this builds to the `public` dir, however for github pages it has to be the `docs` dir so you must run some sort of combination of.

```bash
cd docs \
    && mv CNAME ../ \
    && cd .. \
    && rm -rf docs \
    && cp -r public docs \
    && mv CNAME ./docs
```
