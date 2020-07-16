# Host codelab yourselfe

Change directory and get dependencies:
```
cd tools
npm install
```

Then change directory again, rebuild the site and host codelab:

```
cd site
cd codelabs/ && claat export Using-DashJS-with-TypeScript-in-VSCode.md && cd .. && gulp serve --codelabs-dir=codelabs
```
