<template lang="html">
  <app-wrapper>
    <app-navi/>
    <app-register v-if="todoFilter !== 'completedTodos'" />
    <app-error-message v-if="errorMessage" />
    <template v-slot:todos>
      <app-list v-if="todos.length" :todos="todos" />
      <app-empty-message v-else />
    </template>
  </app-wrapper>
</template>
<!--
３行目に<app-navi/>追加
app-error-mssageにv-if追加
app-empty-messageにv-else追加
-->

<script>
import Wrapper from 'TodoVuexDir/components/Wrapper';
import { ErrorMessage, EmptyMessage } from 'TodoVuexDir/components/Message';
import Register from 'TodoVuexDir/components/Register';
import List from 'TodoVuexDir/components/List';
import Navi from 'TodoVuexDir/components/Navi'; // 追加

export default {
  components: {
    appWrapper: Wrapper,
    appErrorMessage: ErrorMessage,
    appEmptyMessage: EmptyMessage,
    appList: List,
    appRegister: Register,
    appNavi: Navi, // 追加
  },
  computed: {
    todoFilter: function() {
      return this.$store.state.todoFilter;
    },
    todos: function() {
      if (this.todoFilter === 'allTodos') {
        return this.$store.state.todos;
      }
      return this.$store.getters[this.todoFilter];
    },
    errorMessage: function() {
      return this.$store.state.errorMessage;
    },
  },
  watch: {
    todos: function(todos) {
      if (!todos.length) this.$store.dispatch('setEmptyMessage', this.todoFilter);
    },
    $route: function(to) {
      this.$store.dispatch('setTodoFilter', to.name);
    },
  },
  created: function() {
    this.$store.dispatch('getTodos');
    this.$store.dispatch('setTodoFilter', this.$route.name);
  },
};
</script>
