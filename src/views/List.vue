<template>
  <div>
    <h1 class="col">Список задач</h1>

    <div class="row">
      <div class="input-field col s6">
        <select ref="select" v-model="filter">
          <option value="" disabled selected>Выберите статус</option>
          <option value="В работе">В работе</option>
          <option value="Просрочено">Просрочено</option>
          <option value="Завершено">Завершено</option>
        </select>
        <label>Статусы</label>
      </div>
      <button v-if="filter" class="btn" @click="filter = null" style="margin-top: 1.5rem;margin-left: 1rem;">Сбросить фильтр</button>
    </div>

    <hr>

    <table v-if="tasks.length" class="highlight">
      <thead>
      <tr>
        <th>№</th>
        <th>Заголовок</th>
        <th>Дата</th>
        <th>Описание</th>
        <th>Статус</th>
        <th>Открыть</th>
      </tr>
      </thead>
      <tbody>
      <tr v-for="(task, index) in displayTasks"
          :key="task.id"
      >
        <td>{{index + 1}}</td>
        <td>{{task.title}}</td>
        <td>{{new Date(task.date).toLocaleDateString()}}</td>
        <td class="description-table">
          <div class="description-text">{{task.description}}</div>
        </td>
        <td>{{task.status}}</td>
        <td>
          <router-link tag="button" class="btn btn-small" :to="'/task/' + task.id">
            Открыть
          </router-link>
        </td>

      </tr>
      </tbody>
    </table>
    <p v-else>Задач нет</p>
  </div>
</template>

<script>
  export default {
    data: () => ({
      filter: null
    }),
    computed: {
      tasks() {
        return this.$store.getters.tasks
      },
      displayTasks() {
        return this.tasks.filter(t => {
          if (!this.filter) {
            return true
          }
          return t.status === this.filter
        })
      }
    },
    mounted() {
      // eslint-disable-next-line no-undef
      M.FormSelect.init(this.$refs.select);
    }
  }
</script>

<style lang="scss" scoped>
  .description-table {
    max-width: 300px;
  }

  .description-text {
    white-space: nowrap;
    text-overflow: ellipsis;
    overflow: hidden;
  }
  .row .col {
    padding-left: 0;
  }
</style>