### 环境配置
```js
npm install cross-env -D
package.json
"build:prod": "cross-env NODE_ENV=production env_config=prod node build/build.js"
"build:test": "cross-env NODE_ENV=production env_config=test node build/build.js"

build/build.js
var spinner = ora('building for' + process.env.NODE_ENV + 'of' + process.env.env_config + 'mode...')

config/index.js build
prodEnv: require('./prod.env'),

build/webpack.prod.config.js
const env = config.build[process.env.env_config+'Env']
```






