<template>
  <section class="container">
    <h1 class="display-1">Todo App</h1>
    <v-form class="mt-4">
      <v-text-field
        label="本文"
        prepend-icon="mdi-account-edit"
        name="content"
        v-model="content"
        @focus="set_flg"
      />
      <v-card-actions>
        <v-btn class="info" @click="insert">登録</v-btn>
        <v-btn @click="find">検索</v-btn>
      </v-card-actions>
    </v-form>
          <v-list light>
        <v-subheader>Todo List</v-subheader>
        <v-list-item v-for="(todo, index) in display_todos" :key="index" link>
          <v-list-item-content>
            <v-list-item-title>{{ todo.content }}</v-list-item-title>
          </v-list-item-content>
          <v-list-item-content>
            <v-list-item-title>{{ todo.created }}</v-list-item-title>
          </v-list-item-content>
          <v-list-item-action>          
            <v-checkbox v-model="todo.value"></v-checkbox>
          </v-list-item-action>
          <v-list-item-action>
            <v-list-item-action-text @click="remove(todo)">削除</v-list-item-action-text>
          </v-list-item-action>
        </v-list-item>
      </v-list>
  </section>
</template>
<script>
import {mapState} from 'vuex';

export default {
  data: function() {
    return {
      content: '',
      find_flg: false
    }
  },
  computed: {
    ...mapState(['todos']),
    display_todos: function(){
      if(this.find_flg){
        var arr = [];
        var data = this.todos;
        data.forEach(element => {
          if(element.content.toLowerCase().indexOf(this.content.toLowerCase()) > -1){
            arr.push(element);
          }
        });
        return arr;
      }else{
        return this.todos;
      }
    }
  },
  methods: {
    insert: function() {
      this.$store.commit('insert', {content: this.content});
      this.content = '';
    },
    find: function(){
      this.find_flg = true;
    },
    set_flg: function(){
      if(this.find_flg){
        this.find_flg = false;
        this.content = '';
      }
    },
    remove: function(todo){
      this.$store.commit('remove', todo)
    }
  }
}
</script>


