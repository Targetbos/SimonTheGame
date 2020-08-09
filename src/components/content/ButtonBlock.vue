<template>
  <div class="button-block__wrap">
      <span v-for="(btnConfig, idx) in btnsConfig" :class="{'btn--active': btnConfig.isActive}" class="btn" 
      :id="idx" 
      :key="idx"
      :style="{background: btnConfig.bgColor, borderRadius: btnConfig.radius}"
      @click="$emit('btnClick',idx)">
      </span>
  </div>
</template>

<script>
export default {
  name: "ButtonBlock",
  props: {
    btnsConfig: Array,
    isActive: Boolean,
  },
};
</script>

<style lang="scss">
.button-block__wrap {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-flow: wrap;
  width: 16em;
  height: 16em;
  border-radius: 50%;
  overflow: hidden;
  box-shadow: 0 0 0.7rem 0 #333;
}
.btn {
  display: block;
  width: 8em;
  height: 8em;
  opacity: 0.8;
  background: btnColor;
  &:hover {
  }
  &:active {
    box-shadow: inset 0 0 1.2rem 0 #000;
  }
  &--active {
    box-shadow: inset 0 0 1.2rem 0 #000;
  }
}
</style>


