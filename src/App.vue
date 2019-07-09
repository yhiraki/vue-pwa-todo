<template>
<v-app>
  <v-toolbar app>
    <v-toolbar-title class="headline text-uppercase">
      <span>Vue</span>
      <span class="font-weight-light">todo app</span>
    </v-toolbar-title>
    <v-spacer></v-spacer>
    <v-btn
      flat
      href="https://github.com/vuetifyjs/vuetify/releases/latest"
      target="_blank"
      >
    </v-btn>
  </v-toolbar>

  <v-content>
    <Todo/>
  </v-content>
</v-app>
</template>

<script>
import { Vue, Component } from 'vue-property-decorator'
import Todo from './components/Todo.vue'

@Component({
  components: {
    Todo
  }
})
export default class App extends Vue {
}
</script>
