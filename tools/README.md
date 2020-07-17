# Host Codelab Yourself

Change directory and get dependencies:

```sh
cd tools/site
npm install
```

Then rebuild the site and host codelab:

```sh
cd codelabs/ && claat export Using-DashJS-with-TypeScript-in-VSCode.md && cd .. && gulp serve --codelabs-dir=codelabs
```
