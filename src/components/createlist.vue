<template>
<f7-block>
  <f7-list no-hairlines-md id="list-form">
    <f7-list-input v-on:input="validateForm" name="listname" type="text" placeholder="List name" clear-button required validate></f7-list-input>
    <f7-list-input name="listitem" type="text" placeholder="What needs to be done?"></f7-list-input>
  </f7-list>

  <f7-row class="button-row">
    <f7-col>
      <f7-button style="max-width: 150px;" fill @click="createList" :disabled="disabled == 1 ? true : false">Create list</f7-button>
    </f7-col>
    <f7-col>
      <f7-button style="max-width: 120px;" raised @click="addTask">+ Add task</f7-button>
    </f7-col>
  </f7-row>

  <h2 id="list-name-preview"> {{ listName }} </h2>
  <f7-list inset no-hairlines-md id="tasks-preview">
    <f7-list-item id="list-item-preview" v-for="(listItem, index) in listItems">
      {{ index+1 }}. {{ listItem.item }}
      <span @click="removeTodo(index)">
        <f7-icon f7="close" size="25px" color="red"></f7-icon>
      </span>
    </f7-list-item>
  </f7-list>
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
      } else if (this.listItems.length == 0) {
        this.disabled = 1;
      } else {
        this.disabled = 0;
      }
    },
    createList: function() {
      let formData = this.$f7.form.convertToData('#list-form');
      if (formData.listname != '' && formData.listname != undefined && this.listItems.length != 0) {
        var ID = function() {
          return Math.random().toString(36).substr(2, 10);
        };

        let listKey = ID();
        this.createdList = {
          name: formData.listname,
          items: this.listItems,
          id: ID(),
          key: listKey
        };

        firebase.database().ref('lists/' + listKey).set({
          name: this.createdList.name,
          items: this.createdList.items,
          id: this.createdList.id,
          key: this.createdList.key
        });

        this.$$('input.input-with-value').forEach(function(input) {
          input.value = '';
        });

        this.listItems = [];
        this.listName = '';
        this.disabled = 1;
        this.$emit('created', this.createdList);
        this.$f7.tab.show(tab1, true);
      } else {
        return false;
      }
    },
    addTask: function() {
      let formData = this.$f7.form.convertToData('#list-form');
      if (formData.listitem != '' && formData.listitem != undefined) {
        var ID = function() {
          return Math.random().toString(36).substr(2, 10);
        };

        this.listItem = {
          item: formData.listitem,
          done: false,
          id: ID()
        };

        this.listItems.push(this.listItem);
        this.validateForm();
        this.$$('input.input-with-value').forEach(function(input) {
          if (input.name === 'listitem') {
            input.value = '';
          }
        });
      } else {
        return false;
      }
    },
    removeTodo(selectedItem) {
      this.listItems.splice(selectedItem, 1);
      if (this.listItems.length <= 0) {
        this.disabled = 1;
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
