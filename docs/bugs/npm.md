> Run `npm bin --global` to find out where npm installs global executables. Add **that** path to your `PATH` environment variable, by adding a line in your `.profile`/`.bash_profile`/`.zprofile` or similar, depending on which shell you use (you can find out what is your default shell using `echo $SHELL`).

- [x] npm 缓存旧版package.json

在`package.json`中明明有inspec脚本
```json
"scripts": {
  "start": "nodemon index.js",
  "inspect": "NODE_INSPECT_RESUME_ON_START=1 node inspect index.js"
},
```
但`npm run`显示没有
```bash
➜  server git:(master) ✗ npm run
Lifecycle scripts included in server@1.0.0:
  start
    nodemon index.js
➜  server git:(master) ✗ npm cache clean --force
npm warn using --force Recommended protections disabled.
➜  server git:(master) ✗ npm run
Lifecycle scripts included in server@1.0.0:
    start
    nodemon index.js
available via `npm run-script`:
    inspect
    NODE_INSPECT_RESUME_ON_START=1 node inspect index.js
```
