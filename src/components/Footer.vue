<template>
    <footer class="footer">
        <div class="footer__container container">
            <p>{{ copyright }}</p>
        </div>
    </footer>
</template>

<script>
export default {
    name: "Footer",
    components: {},
    props: {
        copyright: String
    }
}
</script>

<style lang="scss">
.footer{
  width:100%;
  background: #333;  
  &__container{
    & p{
      font-size: 0.8rem;
      text-align:center;
      padding: 1rem;
      margin: 1rem;
      color: #fff;
    }
  }
}
</style>
