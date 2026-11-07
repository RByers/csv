# csv

Seamlessly Display and Edit CSVs in Visual Studio Code.

@AGENTS.md

## Running tests on Node 22

`npm test` runs `node --test out/test`, which works on Node 20 but fails on Node 22: newer Node
treats the directory argument as a single test file. On Node 22, compile and pass a glob instead:

```sh
npm run compile && node --test 'out/test/**/*.test.js'
```
