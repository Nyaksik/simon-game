<template>
  <div class="app">
    <div class="app__game">
      <GameField v-model="playerColor" :game="isGame" :color="currentColor" :fields="gameFields" />  
    </div>
    <div class="app__game-settings">
      <button @click="isGame = !isGame">{{ isGame ? 'Рестарт' : 'Старт'}}</button>
      <input name="mode" value="1500" type="radio" v-model="mode">1.5s
      <input name="mode" value="1000" type="radio" checked v-model="mode">1s
      <input name="mode" value="400" type="radio" v-model="mode">0.4s
    </div>
  </div>
</template>

<script>
  import GameField from "@/components/GameField"

  export default {
    components: {
      GameField
    },
    data() {
      return {
        gameFields: [
          {
            id: 1,
            color: "blue"
          },
          {
            id: 2,
            color: "red"
          },
          {
            id: 3,
            color: "yellow"
          },
          {
            id: 4,
            color: "green"
          }
        ],
        mode: 1000,
        isGame: false,
        randomColor: [],
        playerColor: [],
        round: 1,
        currentColor: ''
      }
    },
    methods: {
      setRandomColor() {
        let count = this.round
        while(count !== 0) {
          const randomDigit = Math.floor(Math.random() * 4)
          this.randomColor = [...this.randomColor, this.gameFields[randomDigit].color]
          count -= 1
        }
      },
      timeout(ms) {
        return new Promise(resolve => setTimeout(resolve, ms))
      },
      async flashColor() {
        for(let i = 0; i < this.randomColor.length; i++) {
            this.currentColor = this.randomColor[i]
            await this.timeout(Number(this.mode))
            this.currentColor = ''
            await this.timeout(Number(this.mode))
          }
        }
      },
      watch: {
        isGame() {
          if(this.isGame) {
            this.setRandomColor()
            this.flashColor()
          } else {
            console.log('restart')
            this.randomColor = []
            this.playerColor = []
            this.round = 4
            this.currentColor = ''
          }
        }
      }
  }
</script>

<style lang="scss">
  .app {
    display: flex;
    justify-content: center;
    align-items: center;
    flex-wrap: nowrap;
    &__game {
      margin-right: 100px;
    }
    &__game-settings {
      display: flex;
      flex-direction: column;
      justify-content: center;
      align-items: center;
      & input {
        margin-top: 15px;
      }
    }
  }
</style>