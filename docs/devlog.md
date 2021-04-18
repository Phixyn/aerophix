# AeroPhix Development Log

```
Version: 0.2.0
Description: General aviation flight planner.
Project start: 27 November 2020
GitHub: https://github.com/Phixyn/aerophix
Issue tracker: https://github.com/Phixyn/aerophix/issues

Devlog template: 1.1.0
```

- - -

## Navigation

- [Navigation](#navigation)
- [Todo List](#todo-list)
- [Links](#links)
    - [Guides](#guides)
    - [Vue Router](#vue-router)
    - [Vuetify](#vuetify)
    - [Testing](#testing)
    - [Mapbox](#mapbox)
    - [APIs](#apis)
    - [Packages](#packages)
    - [Tools](#tools)
    - [Inspiration](#inspiration)
- [Notepad](#notepad)
    - [Tools](#tools-1)
    - [The Need For a Proxy Server](#the-need-for-a-proxy-server)
- [Scrap Paper](#scrap-paper)

- - -

## Todo List

> ✅ List of what needs to be done. Try to keep this short and actionable. Most tasks should be in Trello, Jira or whatever the most hip and trendy tool is.

- [ ] Investigate differences between Axios with try..catch and just .then/.catch
  - [ ] Use consistent approach in both projects (also see [examples](https://jasonwatmore.com/post/2020/07/23/vue-axios-http-get-request-examples))
- [ ] Airport list (can be hardcoded for now I guess) - KPWK, CYOO, CZBA
- [ ] Airport page: display basic info (name, etc), METAR tab, TAF tab, NOTAM tab
- [ ] Configure History mode in Vue Router (see [Vue Router](#vue-router))

## Links

> 📚 Help reduce open tabs! Keep all links related to the project here. If a link is relavant to more than one project, do add it to other devlogs too and/or bookmark it. Try to give a brief description of what the webpage has and why it's relevant to the project, and/or why we may find it useful.

### Guides

- [Vue.js REST API Consumption with Axios | DigitalOcean](https://www.digitalocean.com/community/tutorials/vuejs-rest-api-axios)

### Vue Router

- [Routing — Vue.js](https://vuejs.org/v2/guide/routing.html)
- [Getting Started | Vue Router](https://router.vuejs.org/guide/#javascript)
- [HTML5 History Mode | Vue Router](https://router.vuejs.org/guide/essentials/history-mode.html)

### Vuetify

- [Components — Vuetify](https://vuetifyjs.com/en/components/buttons/#sizing)
- https://vuetifyjs.com/en/components/grids/#no-gutters
- https://vuejs.org/v2/guide/computed.html#Watchers

### Testing

- https://www.freecodecamp.org/news/how-to-unit-test-your-first-vue-js-component-14db6e1e360d/
- https://vuejs.org/v2/cookbook/unit-testing-vue-components.html
- https://vuejs.org/v2/guide/testing.html

### Mapbox

- https://dev.to/hmintoh/how-to-mapbox-with-vue-js-2a34
- https://medium.com/swlh/use-mapbox-popups-with-vue-3-7b454387d0d2
- https://soal.github.io/vue-mapbox/guide/
- https://github.com/soal/vue-mapbox/tree/development/src

### APIs

- [Public APIs: A collective list of free APIs for use in software and web development](https://github.com/public-apis/public-apis)
- [AVWX Main Site](https://avwx.rest/)
- [AVWX Account Management](https://account.avwx.rest/)
- [AVWX API Docs](https://avwx.docs.apiary.io/#introduction)

### Packages

- [axios/axios: Promise based HTTP client for the browser and node.js](https://github.com/axios/axios)
  - [Axios Cheat Sheet - Kapeli](https://kapeli.com/cheat_sheets/Axios.docset/Contents/Resources/Documents/index)
- [dotenv  -  npm](https://www.npmjs.com/package/dotenv)
- [kovnick/metar-taf: NPM package that allows you to get TAF and METAR reports](https://github.com/kovnick/metar-taf)

### Tools

- [avwx-rest/avwx-engine: Aviation Weather parsing engine. METAR & TAF](https://github.com/avwx-rest/avwx-engine)
- [METAR-TAF Viewer](https://en.allmetsat.com/metar-taf/north-america.php?icao=KORD)

### Inspiration

- [Aerolink – Let's Fly Together – Aplicación para Pilotos](https://aerolink.club/)

## Notepad

> 📓 A section to keep general notes about the project, scribbles and things that don't really fit in any other section.

### Tools

- Insomnia - REST API client

### The Need For a Proxy Server

The need for an Express server to handle external API requests comes from the use of service accounts.

Secrets (which include things like **private API keys**, sometimes called **service accounts**) should never be used directly in any sort of front-end application. Whether it's Vue, Angular or just jQuery with AJAX/fetch API.

The `.env` file in Vue is appropriate for storing configurations and *public* API keys (an example of this is the Google Maps API key) that can be consumed by Vue, client-side. Anyone can find out those keys if they wanted to.

If you need any access to a private API endpoint or otherwise access a service using a key/secret that your end-users shouldn't have access to, you need to perform that server-side, not in Vue. Alternatively, you could use Nuxt, where the rendering is done server-side, in which case you could leverage private API keys, but in that case, you should be setting your secrets via environmental variables at runtime - they should never be in your source code.

Only references to the location of secrets should be in source.

> Source: https://forum.vuejs.org/t/how-to-hide-api-keys-from-source-code/102974/5

Most of our API usage will most likely require service accounts (for example, [AVWX](https://avwx.rest/)).

## Scrap Paper

> 📝 For code snippets, experimental code, things that need to be moved to a gist, or just temporary code.

