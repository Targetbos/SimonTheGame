<template>
  <div class="modal__wrap" :class="{'modal--visible':dataModal.modalVisible}" @click="$emit('modalHide')">
    <div class="modal__inner">
      <img :src="dataModal.imgUrl" class="modal__img">
      <span class="modal__title">{{ dataModal.title }}</span>
    </div>
  </div>  
</template>
<script>
export default {
  name: "modal",
  props: {
    dataModal: Object,
  },
};
</script>
<style lang="scss">
.modal {
  &__wrap {
    display: flex;
    position: fixed;
    top: 0;
    bottom: 0;
    right: 0;
    left: 0;
    align-items: center;
    justify-content: center;
    background: rgba(0, 0, 0, 0.8);
    transform: translate(-120%, -120%);
    transition: transform 0.5s;
  }
  &--visible {
    transform: translate(0%, 0%);
  }
  &__inner {
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;
    border-radius: 1rem;
    background: #fff;
    width: 30vw;
    height: 20vw;
  }
  &__img {
    display: flex;
    width: 10vw;
    height: 10vw;
    border-radius: 50%;
    overflow: hidden;
    margin: 1em auto;
    border: none;
  }
  &__title {
    font-size: 2rem;
    font-weight: bold;
    margin-bottom: 1em;
  }
}

@media screen and (max-width: $desktopWidth) {
}
@media screen and (max-width: $tableWidth) {
}
@media screen and (max-width: $phoneWidth) {
  .modal {
    &__inner {
      width: 80vw;
      height: auto;
    }
    &__title {
      font-size: 1.7rem;
    }
    &__img {
      width: 20vw;
      height: 20vw;
    }
  }
}
</style>

