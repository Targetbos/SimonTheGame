<template>
  <main class="main container">
    <div class="inner">
        <ButtonBlock :btnsConfig="btnsConfig" @btnClick="getClickBtn"/>
        <ControlPanel :dataGame="dataGame" :dataControls="controlConfig" @setTimeDelay="getTimeDelay" @clickStart="init"/>
        <Modal :dataModal="dataModal" @modalHide="modalHide" />
    </div>
  </main>
</template>

<script>
import ButtonBlock from "./ButtonBlock.vue";
import ControlPanel from "./ControlPanel.vue";
import Modal from "./Modal.vue";

export default {
  name: "Content",
  components: {
    ButtonBlock,
    ControlPanel,
    Modal,
  },
  data() {
    return {
      btnsConfig: [
        { bgColor: "#ff0000", radius: "8rem 0 0 0", isActive: false },
        { bgColor: "#ffc500", radius: "0 8rem 0 0", isActive: false },
        { bgColor: "#2419b2", radius: "0 0 0 8rem", isActive: false },
        { bgColor: "#00cc00", radius: "0 0 8rem 0", isActive: false },
      ],
      controlConfig: [
        { text: "Легкая", delay: "1.5", checked: true },
        { text: "Средняя", delay: "1" },
        { text: "Сложная", delay: "0.4" },
      ],
      dataGame: {
        timeDelay: 1500, // Задержка в мс
        gameOver: false, // Проигрыш
        startGame: false, //статус игры
        round: 0, //раунд
        getArrayBtn: Array, //копия массива кнопок, для проверки
        gameProcess: false, // хранит процесс имитации нажатия кнопок
      },
      clickArray: [],
      dataModal: {
        modalVisible: false,
        imgUrl: String,
        title: String,
      },
    };
  },
  props: {},
  methods: {
    //закрытие моадльного окна
    modalHide: function() {
      this.dataModal.modalVisible = false;
    },
    // Обработка клика на кнопки и сравнение с данными массива
    getClickBtn: function(idx) {
      if (!this.dataGame.startGame) {
        this.dataModal = {
          modalVisible: true,
          imgUrl: "assets/images/start.jpg",
          title: 'Сначала жми "Старт"',
        };
        return;
      }
      if (this.dataGame.gameProcess) {
        return;
      }
      var arrayBtn = this.dataGame.getArrayBtn;
      if (arrayBtn.length > 0) {
        let numBtn = arrayBtn.shift();
        if (idx === numBtn) {
          this.playSound(idx);
        } else {
          this.playSound(4);
          this.gameOver();
        }
      }
      if (arrayBtn.length == 0 && !this.dataGame.gameOver) {
        setTimeout(this.startNewRound, this.dataGame.timeDelay);
      }
    },
    // запускает имитацию нажатия на кнопки из массива
    setClickBtn: async function(arrayBtn) {
      if (!this.dataGame.startGame) {
        return;
      }
      var numBtn = 0;
      var _this = this;
      return new Promise(resolve => {
        var timer = setTimeout(function clickBtn() {
          _this.btnsConfig[numBtn].isActive = false;
          if (arrayBtn.length > 0) {
            numBtn = arrayBtn.shift();
            _this.playSound(numBtn);
            _this.btnsConfig[numBtn].isActive = true;
            timer = setTimeout(clickBtn, _this.dataGame.timeDelay);
          } else {
            _this.dataGame.gameProcess = false;
            resolve();
          }
        });
      });
    },
    // Вызывается при проигрыше
    gameOver: function() {
      this.dataGame.gameOver = true;
      this.dataGame.startGame = false;
      this.dataModal = {
        modalVisible: true,
        imgUrl: "assets/images/gameover.jpg",
        title: "Вы проиграли",
      };
    },
    //Устанавливает время игры
    getTimeDelay: function(time) {
      this.dataGame.timeDelay = time * 1000;
    },
    //воспроизведение звука
    playSound: function(btnIdx) {
      //воспроизводит звук кнопки полученного IDX
      var audio = new Audio();
      audio.src = "assets/audio/track" + btnIdx + ".mp3";
      audio.play();
    },
    //инициализация после нажатина на кнопку "Старт"
    init: function() {
      this.dataGame.gameOver = false;
      this.dataGame.startGame = true;
      this.clickArray.length = 0;

      this.startNewRound();
    },
    //запускает генерацию нового уровня
    startNewRound: async function() {
      this.dataGame.gameProcess = true;
      this.clickArray.push(this.getRandomNum());
      var setArrayBtn = this.clickArray.slice(),
        getArrayBtn = this.clickArray.slice();
      this.dataGame.round = this.clickArray.length;
      this.setClickBtn(setArrayBtn).then(
        result => (this.dataGame.getArrayBtn = getArrayBtn)
      );
    },

    getRandomNum: function() {
      //Генерирует случайное число от 0 до 3 и сравнивает с последним в массиве, если совпадает, то генерирует следующее.
      let randomNum = 0;
      if (this.clickArray.length > 0) {
        let lastNumArray = this.clickArray[this.clickArray.length - 1];
        randomNum = Math.floor(Math.random() * 4);
        if (Number(lastNumArray) === Number(randomNum)) {
          let rundomNoDouble = this.getRandomNum();
          return rundomNoDouble;
        }
        return randomNum;
      } else {
        randomNum = Math.floor(Math.random() * (3 + 1));
        return randomNum;
      }
    },
  },
};
</script>

<style lang="scss" >
.main {
  display: flex;
  align-items: center;
  justify-content: center;
  height: 80vh;
}
.inner {
  display: flex;
  flex-flow: row nowrap;
}
@media screen and (max-width: $desktopWidth) {
  font-size: 16px;
}
@media screen and (max-width: $tableWidth) {
  font-size: 14px;
}
@media screen and (max-width: $phoneWidth) {
  .inner {
    width: 100%;
    flex-flow: column;
    align-items: center;
  }
}
</style>


