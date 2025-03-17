<template>
  <div class="edit-page">
    <h1>Редактирование игроков</h1>

    <p v-if="isEmpty" class="edit-page__empty-message">Игроков нет. Добавьте игроков!</p>

    <div v-else v-for="(item, index) in usersLife" :key="`${item.name}_${index}`" class="edit-page__row">
      <input class="edit-page__input" id="name" v-model="item.name" @input="updatePlayer(item)">
      <div class="edit-page__counter">
        <button class="edit-page__counter-button" @click="minusLife(item)" :disabled="item.life <= 0">-</button>
        <span class="edit-page__counter-life">{{ item.life }}</span>
        <button class="edit-page__counter-button" @click="plusLife(item)">+</button>
      </div>
    </div>

    <div v-if="!isEmpty">
      <h2>Рейтинг</h2>
      <table>
        <tr v-for="(item, index) in rating" :key="index">
          <td :class="{ 'first': index === 0 }">{{ index + 1 }}</td>
          <td :class="{ 'first': index === 0 }" v-html="`У игрока <b>${item.name}</b> ${item.life} жизней`"></td>
        </tr>
      </table>
    </div>
  </div>
</template>

<script>
export default {
  name: 'LifeCounter',

  props: {
    playersList: {
      type: Array,
      required: true,
    },
  },

  computed: {
    isEmpty() {
      return this.playersList.length === 0;
    },
    usersLife() {
      return this.playersList;
    },
    rating() {
      let places = this.usersLife;
      places.sort((a, b) => b.life - a.life);

      return places;
    }
  },

  methods: {
    plusLife(item) {
      item.life++;
    },
    minusLife(item) {
      item.life--;
    },
    updatePlayer(item) {
      this.$emit('update-player', item);
    },
  },
}
</script>

<style lang="scss">
table {
  width: 100%;
  display: flex;
  flex-direction: column;
  gap: 20px;

  .first {
    background-color: #98FB98;
  }

  td {
    padding: 5px 10px;
    border: 1px solid #2c3e50;

    &:last-child {
      width: 100%;
    }
  }
}

.edit-page {
  &__row {
    display: flex;
    align-items: center;
    justify-content: space-between;
    gap: 12px;
    margin-top: 20px;
  }

  &__input {
    width: 70%;
    height: 24px;
  }

  &__counter {
    display: flex;
    align-items: center;

    &-button {
      display: flex;
      align-items: center;
      justify-content: center;
      padding: 10px 20px;
      cursor: pointer;
      border: 1px solid #ccc;
      border-radius: 5px;
      background-color: #f9f9f9;
    }

    &-life {
      margin: 0 12px;
    }
  }
}
</style>