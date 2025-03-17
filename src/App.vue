<template>
  <div>
    <div class="tabs">
      <button v-for="(tab, index) in tabs" :key="index" @click="currentTab = tab.component"
        :class="['tab-button', { active: currentTab === tab.component }]">
        {{ tab.name }}
      </button>
    </div>

    <component :is="currentTab" class="tab-content" @players-list="createdPlayers" v-bind="currentProps"
      @update-player="handleUpdatePlayer" />
  </div>
</template>

<script>
import CreatePlayer from './components/CreatePlayer.vue'
import EditPlayers from './components/EditPlayers.vue'

export default {
  name: 'App',
  components: {
    CreatePlayer,
    EditPlayers
  },
  data() {
    return {
      playersList: [],
      currentTab: 'CreatePlayer',
      tabs: [
        { name: 'CreatePlayer', component: 'CreatePlayer' },
        { name: 'EditPlayers', component: 'EditPlayers' }
      ]
    }
  },
  computed: {
    currentProps() {
      if (this.currentTab === 'EditPlayers') {
        return { playersList: this.playersList };
      }
      return {};
    },
  },

  methods: {
    createdPlayers(list) {
      this.playersList = [...this.playersList, ...list];
    },
    handleUpdatePlayer(updatedPlayer) {
      const index = this.playersList.findIndex((p) => p.name === updatedPlayer.name);
      if (index !== -1) {
        this.playersList.splice(index, 1, updatedPlayer);
      }
    },
  },
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin: 60px auto;
  width: 600px;
}

.tabs {
  display: flex;
  margin-bottom: 20px;
}

.tab-button {
  padding: 10px 20px;
  margin-right: 10px;
  cursor: pointer;
  border: 1px solid #ccc;
  border-radius: 5px;
  background-color: #f9f9f9;

  &.active {
    background-color: #ddd;
  }
}
</style>