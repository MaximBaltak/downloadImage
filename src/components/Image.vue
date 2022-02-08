<template>
  <li class="image"><img :src="image.image" alt="image Added"><button @click="del"></button></li>
</template>

<script>
export default {
  name: 'Image',
  props: ['image'],
  methods: {
    del () {
      this.$emit('delete', this.image.id)
    }
  }
}
</script>

<style scoped lang="scss">
.image{
  position: relative;
  width: 100px;
  height: 100px;
  border-radius: 100%;
  margin: 15px;
  img{
    width: inherit;
    height: inherit;
    border-radius: inherit;
  }
  button{
    position: absolute;
    outline: none;
    border: none;
    width: 25px;
    height: 25px;
    background: url("./../assets/delete.png") center center/cover no-repeat;
    transform: translate(-24px, 4px);
    cursor: pointer;
  }
}

</style>
