<template>
  <div id="app" class="container">
    <h1>Galeria de fotos</h1>
    <hr />
    <FormTodo v-on:add-todo="addImage"></FormTodo>
    <div class="card-columns" style="width: 100%; overflow:auto;">
      <p v-if="array_images.length <= 0">Sem fotos...</p>
      <div v-else class="card" v-for="(objeto_imagem, index) in allImages" v-bind:key="index">
        <img class="card-img-top rounded" v-bind:src="objeto_imagem.image.imageBase" >
        <div class="card-body">
          <h5 class="card-title">{{ objeto_imagem.description }}</h5>
          <p class="card-text">
            <span><b>Nome:</b> {{ objeto_imagem.image.imageName }}</span>
          </p>
          <p class="card-text">
            <span><b>Tipo:</b> {{ objeto_imagem.image.imageType }}</span>
          </p>
          <p class="card-text">
            <span><b>Tamanho:</b> {{ (objeto_imagem.image.imageSize / 1024 ** 2).toFixed(2) }} Mb</span>
          </p>
          <a href="#" title="Excluir" v-on:click.prevent="removeImage(index)">Excluir</a>
        </div>
      </div>
    </div>
    <hr />
  </div>
</template>

<script>
import FormTodo from './FormTodo';
export default {
  components: {
    FormTodo
  },
  data() {
    return {
      array_images: []
    }
  },
  methods: {
    addImage(image) {
      if(image.errors==0){
        this.array_images.push(image);
      }
    },
    removeImage(index) {
      this.array_images.splice(index, 1);
    }
  },
  computed: {
    allImages() {
      return this.array_images.map(image => ({
        ...image,
        description: image.description.trim() === '' ? 'Sem descrição' : image.description
      }))
    }
  },
  watch: {
    array_images(val) {
      console.log('val', val)
    }
  }
}
</script>

<style>
  .grid2{
    display: grid;
    grid-template-columns: 1fr 4fr 1fr;
    margin: 0px;
    padding: 0px;
    grid-template-rows: 2fr 96fr 2fr;
    grid-template-areas: "h h h"
                         "a m m"
                         "f f f"
  }
</style>