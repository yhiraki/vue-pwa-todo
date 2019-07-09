<template>
<v-container>
  <v-layout>
    <v-text-field
      v-model="todoName"
      label="Write a todo."
      append-icon="add"
      @click:append="add"
      @keyup.enter="add"
      />
  </v-layout>

  <v-card>
    <template v-for="(ls, i) in [todos, dones]">
      <v-list subheader v-if="ls.length" :key="i">
        <v-subheader v-if="i===0">Todos</v-subheader>
        <v-subheader v-if="i===1">Dones</v-subheader>

        <v-list-tile
          v-for="item in ls"
          :key="item.id"
          @click="toggleDone(item)"
          >
          <v-list-tile-action>
            <v-checkbox v-model="item.done" @click="toggleDone(item)"/>
          </v-list-tile-action>

          <v-list-tile-content>
            <s v-if="i===1"> {{item.name}} </s>
            <span v-else> {{item.name}} </span>
          </v-list-tile-content>

          <v-list-tile-action>
            <v-icon @click="del(item)">close</v-icon>
          </v-list-tile-action>
        </v-list-tile>
      </v-list>
    </template>
  </v-card>
</v-container>
</template>

<script lang="ts">
import { Vue, Component, Watch } from 'vue-property-decorator'

const storage = localStorage
const KEY = 'todos'

type TodoItem = {
  id: number;
  name: string;
  done: boolean;
};

@Component
export default class Todo extends Vue {
  private list: TodoItem[] = []
  todoName: string = ''
  private lastId = 0

  get dones (): TodoItem[] {
    return this.list.filter(t => t.done)
  }

  get todos (): TodoItem[] {
    return this.list.filter(t => !t.done)
  }

  created () {
    this.load()
  }

  add () {
    if (this.todoName === '') return
    this.list.push({
      id: this.lastId++,
      name: this.todoName,
      done: false
    })
    this.todoName = ''
  }

  del (item: TodoItem) {
    this.list = this.list.filter(t => t.id !== item.id)
  }

  toggleDone (item: TodoItem) {
    item.done = !item.done
  }

  @Watch('list', { deep: true })
  private save () {
    storage.setItem(KEY, JSON.stringify(this.list))
  }

  private load () {
    const value = storage.getItem(KEY)
    if (value) this.list = JSON.parse(value)
  }
}
</script>
