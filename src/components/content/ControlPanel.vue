<template>
  <div class="control-panel__wrap">
    <div class="round block"> Раунд: {{dataGame.round}} </div>
    <div v-if="dataGame.gameOver" class="game-over__text"> Вы проиграли в {{dataGame.round}} раунде</div>
    <span class="start-btn" @click="$emit('clickStart')">Старт</span>
    <div class="game-options__wrap">
      <span class="game-options__title">Сложность игры</span>
        <template v-for="config of dataControls">
          <label>
            <input type="radio" name="game-options" class="game-options__input" 
            :value="config.delay" 
            :checked="config.checked" 
            @change="$emit('setTimeDelay', $event.target.value)">
            <span class="game-options__text">{{config.text}}</span>
          </label>
        </template>
      
    </div>

  </div>
</template>

<script>
export default {
  name: "ControlPanel",
  data() {
    return {};
  },
  props: {
    dataControls: Array,
    dataGame: Object,
  },
};
</script>

<style lang="scss" scoped>
.control-panel__wrap {
  display: flex;
  flex-direction: column;
  justify-content: center;
  width: 14rem;
  border: 1px dotted #333;
  box-sizing: border-box;
  padding: 0.5rem;
  margin: 2rem;
}
.block {
  display: block;
  width: 100%;
  text-align: center;
}
.round {
  margin-bottom: 1rem;
  font-weight: 700;
}
.start-btn {
  display: flex;
  justify-content: center;
  padding: 0.3rem 0.7rem;
  border-radius: 0.2rem;
  background: #331188;
  color: #fff;
  margin: 0.3rem;
  transition: background 0.3s;
  box-shadow: 0 0 0.3rem 0 #333;
  cursor: pointer;
  &:hover {
    background: #00cc00;
    text-shadow: 0 0 0.7rem 0 #000;
  }
  &:active {
    box-shadow: inset 0 0 0.3rem 0 #333;
  }
}
.game-options {
  &__wrap {
    display: flex;
    flex-direction: column;
    width: 100%;
    margin-top: 1rem;
    padding-left: 2rem;
  }
  &__title {
    font-size: 1rem;
    font-weight: bold;
  }
  &__input {
    border: 1px solid #333;
  }
  &__text {
    font-size: 1rem;
  }
}
.game-over__text {
  font-size: 90%;
  font-weight: bold;
  text-align: center;
  margin-bottom: 0.6rem;
}
</style>


