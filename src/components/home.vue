<template>
<f7-block v-if="!listSelected">
  <h2 id="no-list">No list is selected.</h2>
</f7-block>
<f7-block v-else>
</f7-block>
</template>

<script>
export default {
  data() {
    return {
      listSelected: false
    }
  },
  computed: {
  },
  watch: {
  },
  methods: {
    checkTask: function(task) {
    },
    deleteList: function(list) {
    }
  }
}
</script>

<style media="screen" scoped>
#list-title {
  padding: 0 20px;
  margin-top: 0;
}

#tasks {
  margin-left: 0;
}

.checked {
  color: #ababab;
}

#title-row {
  margin-bottom: -20px;
  margin-right: 20px;
}

#no-list {
  padding: 0 20px;
}
</style>
