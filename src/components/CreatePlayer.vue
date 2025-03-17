<template>
  <div class="create-page">
    <h1>Добавить нового игрока</h1>
    <div class="create-page__row">
      <input class="create-page__name" id="name" type="text" v-model="playersName" placeholder="Имя" />
      <input class="create-page__life" id="life" type="number" v-model="playersLife" placeholder="Жизней" />
      <button class="create-page__button" type="button" :disabled="!isFormValid" @click="createPlayer">
        Создать
      </button>
    </div>
  </div>
</template>


<script>
export default {
  name: 'CreatePlayer',

  data() {
    return {
      players: [],
      playersName: '',
      playersLife: ''
    };
  },
  computed: {
    isFormValid() {
      return this.playersName?.trim() !== '' && this.playersLife > 0;
    },
  },
  methods: {
    validateForm() {
      if (this.playersName?.trim() === '' || this.playersName === undefined) {
        alert('Укажите имя');
        return false;
      }

      if (this.playersLife === '' || this.playersLife === undefined) {
        alert('Укажите количество жизней');
        return false;
      }

      if (this.playersLife <= 0) {
        alert('Значение жизней должно быть больше нуля');
        return false;
      }

      return true;
    },
    createPlayer() {
      if (!this.validateForm()) {
        return;
      }

      this.players.push({
        'name': this.playersName,
        'life': this.playersLife,
      })

      this.$emit('players-list', this.players);
      this.resetForm();
      alert('Добавлен новый пользователь');
    },
    resetForm() {
      this.playersName = '';
      this.playersLife = '';
    },
  },
}
</script>

<style lang="scss">
.create-page {
  &__row {
    display: flex;
    gap: 12px;
    margin-top: 20px;
    align-items: center;
  }

  &__name {
    width: 100%;
    height: 24px;
  }

  &__life {
    width: 70px;
    height: 24px;
  }

  &__button {
    height: 30px;
    cursor: pointer;
    border: 1px solid #ccc;
    border-radius: 5px;
    background-color: #f9f9f9;
  }
}
</style>
