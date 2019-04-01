<template>
<f7-block>
  <f7-list no-hairlines-md id="list-form">
    <f7-list-input v-on:input="validateForm" name="listname" type="text" placeholder="List name" clear-button required validate></f7-list-input>
    <f7-list-input name="listitem" type="text" placeholder="What needs to be done?"></f7-list-input>
  </f7-list>

  <f7-row class="button-row">
    <f7-col>
      <f7-button style="max-width: 150px;" fill @click="" :disabled="disabled == 1 ? true : false">Create list</f7-button>
    </f7-col>
    <f7-col>
      <f7-button style="max-width: 120px;" raised @click="">+ Add task</f7-button>
    </f7-col>
  </f7-row>
</f7-block>
</template>

<script>
export default {
  data() {
    return {
      listName: '',
      listItems: [],
      listItem: {},
      createdList: {},
      lists: [],
      listid: 0,
      disabled: 1
    }
  },
  methods: {
    validateForm: function() {
      let formData = this.$f7.form.convertToData('#list-form');
      this.listName = formData.listname;
      if (formData.listname == '') {
        this.disabled = 1;
      } else {
        this.disabled = 0;
      }
    },
  }
}
</script>

<style media="screen" scoped>
.button-row {
  padding: 0 30px;
  margin-top: -10px;
}

#warning-text {
  padding-left: 35px;
  margin-top: 5px;
  color: red;
  z-index: 1;
}

#list-name-preview {
  padding-left: 20px;
  padding-top: 10px;
  margin-bottom: -30px;
}

#tasks-preview {
  margin-left: 0;
}
</style>
