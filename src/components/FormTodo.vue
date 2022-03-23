<template>
  <div class="form-todo form-group">
  <p>
    <input placeholder="Descrição da imagem" type="text" name="imagem_description" class="form-control" v-model="description" />
  </p>
  <p>
    <input type="file" ref="images" name="input-images" class="form-control-file" accept="image/jpeg, image/png" v-on:change="saveImageForRead" multiple/>
  </p>
  <button v-on:click="addImage" type="submit" class="btn btn-primary">Adicionar na galeria</button>
  <p v-if="errors.length">
    <b>Por favor, corrija o(s) seguinte(s) erro(s):</b>
    <ul>
      <li v-for="error in errors" v-bind:key="error">{{ error }}</li>
    </ul>
  </p>
  </div>
</template>

<script>
  export default {
    data() {
      return {
        description: '',
        image: {
          imageName: '',
          imageType: '',
          imageSize: '',
          imageBase: ''
        },
        errors: []
      }
    },
    methods: {
      addImage() {

        this.errors = [];

        if (this.description.trim() === '') {
          this.errors.push("A descrição é obrigatória.");
        }
        if (this.image.imageBase === '') {
          this.errors.push("A imagem é obrigatória.")
        }

        this.$emit('add-todo', {
          description: this.description,
          image: this.image,
          errors: this.errors
        });

        if(this.errors.length==0){
          this.description = '';
          this.image = {};
        }
        
      },
      saveImageForRead(event) {
        
        const fileReader = new FileReader();
        fileReader.readAsDataURL(event.target.files[0]);
        fileReader.onload = () =>{
          this.image.imageBase = fileReader.result;
          this.image.imageName = event.target.files[0].name;
          this.image.imageType = event.target.files[0].type;
          this.image.imageSize = event.target.files[0].size;
        };

      }
    }
  }
</script>