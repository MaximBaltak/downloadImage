<template>
  <div class="form">
    <p v-if="status">Загрузка не удалась</p>
    <div class="wrapper">
      <input @change="add" id="input-image" type="file" accept="image/jpeg, image/png">
      <label class="download" for="input-image"><img src="./assets/add_image.png" alt="add"></label>
    </div>
    <ul class="images">
      <Image @delete="deleteId" v-for="image in images" :key="image.id" :image="image"></Image>
    </ul>
    <button @click="download" class="add">Добавить</button>
  </div>
</template>

<script>
import Image from './components/Image'

export default {
  name: 'App',
  components: { Image },
  data () {
    return {
      images: [],
      status: false
    }
  },
  methods: {
    add (e) {
      const reader = new FileReader()
      reader.readAsDataURL(e.target.files[0])
      reader.onload = () => {
        const body = {
          id: Math.floor(Math.random() * 1000),
          image: reader.result
        }
        this.images.push(body)
        this.status = false
      }
      reader.onerror = () => {
        this.status = true
      }
    },
    deleteId (id) {
      this.images.forEach((image, i) => {
        if (image.id === id) {
          this.images.splice(i, 1)
        }
      })
    },
    download () {
      fetch('./request.txt', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json'
        },
        body: JSON.stringify(this.images)
      }).then(() => {
        alert('Изображения добавлены')
        this.images = []
      })
        .catch(() => {
          this.status = true
        })
    }
  }
}
</script>

<style lang="scss">
#app {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #ffffff;
}

.form {
  width: 600px;
  min-height: 200px;
  margin: 100px auto;
  padding: 10px;
  border-radius: 20px;
  box-shadow: 0 0 10px rgba(0, 0, 0, .5);
}

.wrapper {
  position: relative;
  margin: 0 auto;
  width: 100px;

  input[type='file'] {
    display: none;
  }

  .download {
    display: flex;
    align-items: center;
    justify-content: center;
    position: relative;
    width: 75px;
    height: 75px;
    border-radius: 100%;
    background: #a7a7a7;
    box-shadow: 6px 4px 31px rgba(0, 0, 0, 0.5);
    cursor: pointer;
    transition: all .3s;

    img {
      width: 60px;
      height: 60px;
      opacity: 0.4;
    }

    &:hover {
      transform: translate(-10px, -10px) scale(1.1);
    }
  }
}

.images {
  list-style-type: none;
  padding: 0;
  align-items: center;
  width: 100%;
  margin: 30px auto;
  display: flex;
  flex-wrap: wrap;
}

.add {
  display: block;
  margin: 30px auto;
  background: #005b98;
  width: 200px;
  height: 72px;
  border-radius: 100%;
  border: none;
  outline: none;
  color: white;
  font-size: 18px;
  font-weight: bold;
  transition: all .3s;
  cursor: pointer;

  &:hover {
    box-shadow: 0 0 20px #005b98;
  }
}
</style>
