<template>
<f7-app :params="f7params">
  <f7-statusbar></f7-statusbar>

  <f7-panel left cover>
    <f7-view>
      <f7-page>
        <f7-navbar title="Lists"></f7-navbar>
        <f7-block v-if="lists.length === 0">There are no lists.</f7-block>

        <div class="list-length" v-else>
          <f7-block v-if="lists.length === 1" class="list-length">
            You have <b>{{ lists.length }}</b> list.
          </f7-block>
          <f7-block v-else-if="lists.length != 0" class="list-length">
            You have <b>{{ lists.length }}</b> lists.
          </f7-block>
          <f7-list no-hairlines-md>
            <f7-list-item v-for="list in lists" @click="selectList(list)">
              <span style="width: 65%;"> {{ list.name }} </span>
              <span :class="{ 'all-checked': list.checked.length === list.items.length }">
                {{ list.checked.length }} / {{ list.items.length }}
              </span>
              <f7-icon f7="chevron_right" size="20px"></f7-icon>
            </f7-list-item>
          </f7-list>
        </div>
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
          <home-page :todoList="todoList" :lists="lists" @checked="checkedTasks"></home-page>
        </f7-tab>
        <f7-tab id="tab2" class="page-content">
          <create-list @created="getList"></create-list>
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
    getList: function(list) {
      this.todoList = {
        id: list.id,
        name: list.name,
        items: list.items,
        checked: []
      };

      this.lists.push(this.todoList);
    },
    selectList: function(selection) {
      this.todoList = selection;
      this.$f7.panel.close();
    },
    checkedTasks: function(list) {
      this.todoList.checked = list;
    }
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
