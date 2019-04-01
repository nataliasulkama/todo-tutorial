<template>
<f7-app :params="f7params">
  <f7-statusbar></f7-statusbar>

  <f7-panel left cover>
    <f7-view>
      <f7-page>
        <f7-navbar title="Lists"></f7-navbar>
        <f7-block>Lists will be here :)</f7-block>
      </f7-page>
    </f7-view>
  </f7-panel>

  <f7-view main class="safe-areas" url="/">
    <f7-page :page-content="false">
      <f7-navbar :sliding="false">
        <f7-nav-left>
          <f7-link icon-ios="f7:menu" icon-md="material:menu" panel-open="left"></f7-link>
        </f7-nav-left>
        <f7-nav-title sliding>TODO Lists</f7-nav-title>
      </f7-navbar>

      <f7-toolbar bottom tabbar id="toolbar">
        <f7-link class="bottom-tab" tab-link="#tab1" tab-link-active>
          Home
        </f7-link>
        <f7-link class="bottom-tab" tab-link="#tab2">
          Create
        </f7-link>
      </f7-toolbar>

      <f7-tabs class="tabs" swipeable>
        <f7-tab id="tab1" class="page-content" tab-active>
          <home-page></home-page>
        </f7-tab>
        <f7-tab id="tab2" class="page-content">
          <create-list></create-list>
        </f7-tab>
      </f7-tabs>

    </f7-page>
  </f7-view>

</f7-app>
</template>
<script>
import Home from './home'
import CreateList from './createlist'

export default {
  data() {
    return {
      f7params: {
        name: 'Todo',
        theme: 'auto',
        serviceWorker: {
          path: '/service-worker.js',
        },
      },
      todoList: {},
      lists: [],
    }
  },
  components: {
    'home-page': Home,
    'create-list': CreateList
  },
  methods: {
  }
}
</script>

<style media="screen" scoped>
.list-length {
  margin: 15px 5px -25px 5px;
}

.all-checked {
  color: green;
}
</style>
