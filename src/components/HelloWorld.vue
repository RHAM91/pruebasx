<template>
  <div class="hello">
    <h1>
      Nueva version
    </h1>
    <p>
      For a guide and recipes on how to configure / customize this project,<br>
      check out the
      <a href="https://cli.vuejs.org" target="_blank" rel="noopener">vue-cli documentation</a>.
    </p>
    <h3>Installed CLI Plugins</h3>
    <ul>
      <li><a href="https://github.com/vuejs/vue-cli/tree/dev/packages/%40vue/cli-plugin-babel" target="_blank" rel="noopener">babel</a></li>
      <li><a href="https://github.com/vuejs/vue-cli/tree/dev/packages/%40vue/cli-plugin-router" target="_blank" rel="noopener">router</a></li>
      <li><a href="https://github.com/vuejs/vue-cli/tree/dev/packages/%40vue/cli-plugin-vuex" target="_blank" rel="noopener">vuex</a></li>
    </ul>
    <h3>Essential Links</h3>
    <ul>
      <li><a href="https://vuejs.org" target="_blank" rel="noopener">Core Docs</a></li>
      <li><a href="https://forum.vuejs.org" target="_blank" rel="noopener">Forum</a></li>
      <li><a href="https://chat.vuejs.org" target="_blank" rel="noopener">Community Chat</a></li>
      <li><a href="https://twitter.com/vuejs" target="_blank" rel="noopener">Twitter</a></li>
      <li><a href="https://news.vuejs.org" target="_blank" rel="noopener">News</a></li>
    </ul>
    <h3>Ecosystem</h3>
    <ul>
      <li><a href="https://router.vuejs.org" target="_blank" rel="noopener">vue-router</a></li>
      <li><a href="https://vuex.vuejs.org" target="_blank" rel="noopener">vuex</a></li>
      <li><a href="https://github.com/vuejs/vue-devtools#vue-devtools" target="_blank" rel="noopener">vue-devtools</a></li>
      <li><a href="https://vue-loader.vuejs.org" target="_blank" rel="noopener">vue-loader</a></li>
      <li><a href="https://github.com/vuejs/awesome-vue" target="_blank" rel="noopener">awesome-vue</a></li>
    </ul>
    <h1>
      {{version}}
    </h1>
    <button type="button" v-if="update" @click="pushversion">Actualizar</button>
    <button type="button" @click="descargarNuevoModulo">Ejecutar</button>

  </div>
</template>

<script>

import { ipcRenderer } from 'electron'
window.ipcRenderer = ipcRenderer

export default {
  name: 'HelloWorld',
  data() {
    return {
      version: '',
      show: false,
      update: false
    }
  },
  methods: {
    getVersion(){
      ipcRenderer.send('app_version')

      ipcRenderer.on('app_version', (event, args)=>{
        ipcRenderer.removeAllListeners('app_version')
        this.version = args.version
      })

      ipcRenderer.on('actualizacion', (event, message)=>{
        this.update = message
      })
    },
    pushversion(){
      ipcRenderer.send('ok_update')
    },
    ejecutar(){
      ipcRenderer.send('IniciandoModuloAPI')
    },
    descargarNuevoModulo(){
      ipcRenderer.send('ActualizarModulo')
    }
  },
  mounted() {
    this.getVersion()
    this.ejecutar()
  },
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
