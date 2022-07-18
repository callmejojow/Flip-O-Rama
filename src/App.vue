<template>
  <h1 class="sr-only">Flip-O-Rama</h1>
    <img src="../public/image/flip-o-rama-title.png" alt="Flip-O-Rama" class="title">
  <section class="description">
    <p>Welcome to Flip-O-Rama</p>
    <p>A fun game created by Jolie Wang</p>
    <p>For my dearest son Edward</p>
  </section>
  <transition-group tag="section" class="game-board" name="shuffle-card">
    <AllCard 
      v-for="card in cardList" 
      :key="`${card.value}-${card.variant}`" 
      :value="card.value"
      :visible="card.visible"
      :position="card.position"
      :matched="card.matched"
      @select-card="flipCard"
    />
  </transition-group>
  <h2>{{ status }}</h2>
  <button v-if="newPlayer" @click="startGame" class="button"><img src="../public/image/play.svg"/> Start Game</button>
  <button v-else @click="restartGame" class="button"><img src="../public/image/restart.svg"/> Restart Game</button>
</template>

<script>
import _ from 'lodash'
import { ref,watch,computed } from 'vue'
import AllCard from './components/AllCard'

import { launchConfetti } from './utilities/confetti'

/* eslint-disable */

export default {
  name: 'App',
  components: {
    AllCard
  },
  setup(){
    const cardList = ref([])
    const userSelection = ref([])
    const newPlayer = ref(true)

    const startGame = () =>{
      newPlayer.value = false
      restartGame()
    }

    const status = computed(()=>{
      if(remainingPairs.value === 0){
        return 'You win!'
      } else {
        return `Remaining Pairs: ${remainingPairs.value}` 
      }
    })
    const remainingPairs = computed(()=>{

      const remainingCards = cardList.value.filter
        (card => card.matched === false).length

      return remainingCards / 2
    })

    const restartGame = () => {
      cardList.value = _.shuffle(cardList.value)

      cardList.value = cardList.value.map((card, index) =>{
        return {
          ...card,
          matched: false,
          position: index,
          visible: false,
        }
      })
    }

    const cardItems = ['bat','candy','cauldron','cupcake','moon','ghost','pumpkin','witch-hat']

    cardItems.forEach(item => {
       cardList.value.push({
        value: item,
        variant: 1,
        visible: false,
        position: null,
        matched:false
      })
       cardList.value.push({
        value: item,
        variant: 2,
        visible: true,
        position: null,
        matched:false
      })
    })

    cardList.value = cardList.value.map((card,index) =>{
      return {
        ...card,
        position:index
      }
    })
    const flipCard = (payload) => {
      cardList.value[payload.position].visible = true

      if (userSelection.value[0]){
        if (userSelection.value[0].position === payload.position 
            && userSelection.value[0].faceValue === payload.faceValue
        )
         {
           return
        } 
        else {
        userSelection.value[1] = payload
        }
      } else {
        userSelection.value[0] = payload
      } 
    }

    watch(
      userSelection,
      currentValue => {
        if(currentValue.length === 2) {

          const cardOne = currentValue[0]
          const cardTwo = currentValue[1]
          if(cardOne.faceValue === cardTwo.faceValue){
            cardList.value[cardOne.position].matched = true
            cardList.value[cardTwo.position].matched = true
          } else {
            setTimeout(() => {
              cardList.value[cardOne.position].visible = false
              cardList.value[cardTwo.position].visible = false
            },1000)
          }

          userSelection.value.length = 0
        }
      },
      { deep:true }
    )
    watch(remainingPairs, currentValue => {
      if (currentValue === 0){
        launchConfetti()
      }
    })
    return {
      cardList,
      flipCard,
      userSelection,
      status,
      remainingPairs,
      restartGame,
      startGame,
      newPlayer,
    }
  }
}
</script>


<style>

body {
  margin: 0;
  padding: 0;
  height: 100%;
}

html {
  margin-top: 0;
  height: 100%;
}

a {
  color: white;
  text-decoration: none;
}

a:hover {
  text-decoration: underline;
}

h1 {
  margin-top: 0;
}

h2 {
  font-family: 'Bebas Neue', cursive;
  font-size: 32px;
  letter-spacing: 1.25px;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  padding-top: 60px 0;
  color: #2c3e50;
  background-image: url('../public/image/page-bg.png');
  background-color: #00070c;
  background-position: center;
  background-repeat: no-repeat;
  background-size: cover;
  height: 100%;
  color: #fff;
}

/*.status {
  font-family: 'Titillium Web', sans-serif;
  font-size: 18px;
  text-transform: uppercase;
}*/

.game-board{
  display: grid;
  grid-template-columns:repeat(4, 120px);
  grid-template-rows:repeat(4, 120px);
  grid-column-gap: 24px;
  grid-row-gap: 24px;
  justify-content: center;
}

@media screen and (min-width: 500px) {
  .game-board {
    grid-template-columns: repeat(4, 90px);
    grid-template-rows: repeat(4, 90px);
  }
  .title {
    width: 300px;
  }
}
@media screen and (min-width: 600px) {
  .game-board {
    grid-template-columns: repeat(4, 120px);
    grid-template-rows: repeat(4, 120px);
  }

  .title {
    width: 400px;
  }
}

.title{
  padding-top: 30px;
}

p{
  color: gray;
  letter-spacing: 1px;
  font-size: 18px;
}

.shuffle-card-move {
  transition: transform 0.8s ease-in;
}

.description {
  font-family: 'Bebas Neue', cursive;
  margin-bottom: 30px;
}

.button{
  background-color: orange;
  color: white;
  padding: 0.75rem 1rem;
  display: flex;
  align-items: center;
  justify-content: center;
  margin: 0 auto;
  font-family: 'Bebas Neue', cursive;
  font-size: 1.2rem;
  border: 0;
  border-radius: 10px;
}

.button img {
 padding-right: 5px;
}

.button:hover {
 background-color: #F97613;
}

.button:focus {
 background-color: #F96B00;
}
.sr-only{
  position: absolute;
  width: 1px;
  height: 1px;
  padding: 0;
  margin: -1px;
  overflow: hidden;
  clip: rect(0,0,0,0);
  border: 0;
}
</style>
