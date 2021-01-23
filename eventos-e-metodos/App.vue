<template>
  <div>
    <h1>Minha lista de tarefas:</h1>
    <button @click="handleShowHideList">
      Ver a lista
    </button>
    <br>

    <input 
      type="text"
      @keyup.enter="addTask"
      v-focus 
      v-model="currentTask">

    <ul v-if="showList">
      <li v-for="(task, index) in tasks"
      @dblclick="complete(task)"
      :key="`${task}-${index}`"
      class="task-item"
      :class="{
        'line_through': task.isDone
        }"
      >
        {{ task.name }}
        <button
        @click="remove(task)"
        >&times;</button>
      </li>
    </ul>
    <p v-else> Lista de tarefas escondidas</p>

  </div>
</template>

<script>
const focus = {
  inserted: (el) => {
    el.focus()
  }
}
export default {
  directives: {
    focus
  },
 data: () => ({
   currentTask: '',
   showList: false,
   tasks: [
     {name: 'Fazer o curso', isDone: false}
   ]
 }),
 methods: {
   complete (task) {
    this.tasks = this.tasks.map(t => { 
      if (t.name === task.name) {
        return { ...t, isDone: !t.isDone }
      }
      return {...t}
    })
   },
   handleShowHideList () {
     this.showList = !this.showList
   },
   remove (task) {
     this.tasks = this.tasks.filter(t => t.name !== task.name)
   },
   addTask () {
     this.tasks.push({ 
       name: this.currentTask,
       isDone: false
     })
     this.currentTask = ''
   },
 }
}
</script>

<style scoped>
.line_through {
    text-decoration: line-through;
  }
.task-item {
cursor: pointer;
}

</style>