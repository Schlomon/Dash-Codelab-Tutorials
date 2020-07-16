# Host codelab yourselfe

Change directory and get dependencies:

```
cd tools/site
npm install
```

Then rebuild the site and host codelab:

```
cd codelabs/ && claat export Using-DashJS-with-TypeScript-in-VSCode.md && cd .. && gulp serve --codelabs-dir=codelabs
```
