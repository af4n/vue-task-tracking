<template>
  <div class="row">
    <div v-if="task" class="col s8 offset-s2">
      <h1>{{task.title}}</h1>

      <form @submit.prevent="submitHandler">
        <div class="input-field">
          <textarea v-model="description" id="description" class="materialize-textarea" data-length="2048" style="min-height: 100px;"></textarea>
          <label for="description">Описание</label>
          <span class="character-counter" style="float: right; font-size: 12px;">{{description.length}}/2048</span>
        </div>

        <div class="chips" ref="chips"></div>

        <input type="text" class="datepicker" ref="datepicker">

        <div v-if="task.status !== 'Завершено'">
          <button class="btn" style="margin-right: 1rem;" type="submit">Обновить</button>
          <button class="btn" type="button" @click="completeTask">Завершить</button>
        </div>
      </form>

    </div>
    <p v-else>Задача не найдена</p>
  </div>
</template>

<script>
  export default {
    computed: {
      task() {
        return this.$store.getters.taskById(+this.$route.params.id)
      }
    },
    data: () => ({
      description: '',
      chips: null,
      date: null
    }),
    mounted() {
      this.description = this.task.description
      // eslint-disable-next-line no-undef
      this.chips = M.Chips.init(this.$refs.chips, {
        placeholder: 'Tag name',
        data: this.task.tags
      })
      // eslint-disable-next-line no-undef
      this.date = M.Datepicker.init(this.$refs.datepicker, {
        format: 'dd.mm.yyyy',
        defaultDate: new Date(this.task.date),
        setDefaultDate: true
      })
      setTimeout(() => {
        // eslint-disable-next-line no-undef
        M.updateTextFields()
      }, 0)
    },
    methods: {
      submitHandler() {
        this.$store.dispatch('updateTask', {
          id: this.task.id,
          description: this.description,
          date: this.date.date
        })
        this.$router.push('/list')
      },
      completeTask() {
        this.$store.dispatch('completeTask', this.task.id)
        this.$router.push('/list')
      }
    },
    destroyed() {
      if (this.chips && this.chips.destroy) {
        this.chips.destroy()
      }

      if (this.date && this.date.destroy) {
        this.date.destroy()
      }
    }
  }
</script>

<style lang="scss" scoped>

</style>