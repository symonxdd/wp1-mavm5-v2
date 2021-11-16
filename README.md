<p align="center"><a href="https://vuejs.org" target="_blank" rel="noopener noreferrer"><img width="100" src="https://vuejs.org/images/logo.png" alt="Vue logo"></a></p>

<p align="center">
  <a href="https://circleci.com/gh/vuejs/vue/tree/dev"><img src="https://img.shields.io/circleci/project/github/vuejs/vue/dev.svg?sanitize=true" alt="Build Status"></a>
  <a href="https://npmcharts.com/compare/vue?minimal=true"><img src="https://img.shields.io/npm/dm/vue.svg?sanitize=true" alt="Downloads"></a>
  <a href="https://www.npmjs.com/package/vue"><img src="https://img.shields.io/npm/v/vue.svg?sanitize=true" alt="Version"></a>
  <a href="https://www.npmjs.com/package/vue"><img src="https://img.shields.io/npm/l/vue.svg?sanitize=true" alt="License"></a>  
  <br>
</p>

## Serve
### Serve using Vue CLI
One line: `npm install && npm install -g @vue/cli && vue upgrade && npx vue-cli-service serve`, or:

1. Install dependencies `npm install`
2. Install Vue CLI `npm install -g @vue/cli`
3. Upgrade Vue plugins to latest version `vue upgrade`
4. Actually serve the app `npx vue-cli-service serve`  
5. Navigate to the printed link

### Serve using Vue UI
1. Install dependencies `npm install`
2. Install Vue CLI `npm install -g @vue/cli`
3. Upgrade Vue plugins to latest version `vue upgrade`
4. Open Vue UI `vue ui`
5. Click 'Import' at the top-right corner
6. Navigate to project root
7. Hit 'Import this folder'
8. Left navigation > Tasks > serve
9. Click on 'Run task'
10. Wait for project compilation
11. Hit 'Open app'

## JSON Server
1. `npm install -g json-server`
2. `json-server --watch src/assets/database/afspraken.json`

###### npm scripts
The serve and json-server commands are also available as scripts in `package.json`.
