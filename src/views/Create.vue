<template>
  <div class="row">
    <div class="col s8 offset-s2">
      <h1>Создать новую задачу</h1>

      <form @submit.prevent="submitHandler">
        <div class="input-field">
          <input v-model="title" id="title" type="text" class="validate" required>
          <label for="title">Название</label>
          <span class="helper-text" data-error="Укажите название задачи"></span>
        </div>

        <div class="input-field">
          <textarea v-model="description" id="description" class="materialize-textarea" data-length="2048"></textarea>
          <label for="description">Описание</label>
          <span class="character-counter" style="float: right; font-size: 12px;">{{description.length}}/2048</span>
        </div>

        <div class="chips" ref="chips"></div>

        <input type="text" class="datepicker" ref="datepicker">

        <button class="btn teal lighten-1" type="submit" style="margin-top: 1rem;">Создать</button>
      </form>
    </div>
  </div>
</template>

<script>
export default {
  name: 'create',
  data: () => ({
    description: '',
    title: '',
    chips: null,
    date: null
  }),
  mounted() {
    // eslint-disable-next-line no-undef
    this.chips = M.Chips.init(this.$refs.chips, {
      placeholder: 'Название тега'
    })
    // eslint-disable-next-line no-undef
    this.date = M.Datepicker.init(this.$refs.datepicker, {
      format: 'dd.mm.yyyy',
      defaultDate: new Date(),
      setDefaultDate: true
    });
  },
  methods: {
    submitHandler() {
      const task = {
        title: this.title,
        description: this.description,
        id: Date.now(),
        status: 'В работе',
        tags: this.chips.chipsData,
        date: this.date.date
      }

      this.$store.dispatch('createTask', task)
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
