<p align="center"><a href="https://vuejs.org" target="_blank" rel="noopener noreferrer"><img width="100" src="https://vuejs.org/images/logo.png" alt="Vue logo"></a></p>

<p align="center">
  <a href="https://circleci.com/gh/vuejs/vue/tree/dev"><img src="https://img.shields.io/circleci/project/github/vuejs/vue/dev.svg?sanitize=true" alt="Build Status"></a>
  <a href="https://npmcharts.com/compare/vue?minimal=true"><img src="https://img.shields.io/npm/dm/vue.svg?sanitize=true" alt="Downloads"></a>
  <a href="https://www.npmjs.com/package/vue"><img src="https://img.shields.io/npm/v/vue.svg?sanitize=true" alt="Version"></a>
  <a href="https://www.npmjs.com/package/vue"><img src="https://img.shields.io/npm/l/vue.svg?sanitize=true" alt="License"></a>  
  <br>
</p>

##### 1. RESTful requests via Fetch API
- http://localhost:3000/afspraken/
- http://localhost:3000/afspraken?patient_id=${id}

##### 2. Client-sided validatie
- src\components\AddAfspraak.vue **Ln 66 -> 104** & **Ln 106 -> 141**
- src\components\EditAfspraak.vue **Ln 102 -> 142** & **Ln 182 -> 210 (extra laag)**

##### 3. Vue-files
- ###### src\components
  - AddAfspraak.vue
  - Alert.vue
  - EditAfspraak.vue
  - EditAfspraak.vue
  - Heading.vue
  - Navigation.vue

- ###### src\views
  - Afspraken.vue
  - Home.vue

- ###### src
  - App.vue

##### 4. Vue-router
- src\App.vue **Ln 6**
- src\router\index.js

##### 5. Vuex (niet geïmplementeerd, wel plugin geïnstalleerd)
- src\store\index.js **Ln 6**

## Serve
1. `npm install`
2. `npm install -g @vue/cli`
3. `vue upgrade`
4. `vue-cli-service serve` (script available in package.json)
5. Navigate to the printed link

## JSON Server
1. `npm install -g json-server`
2. `json-server --watch src/assets/database/afspraken.json` (script available in package.json)
