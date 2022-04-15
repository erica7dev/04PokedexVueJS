<template>
  <div id="pokemon">
    <div class="card">
      <div class="card-image">
        <figure>
          <img :src="currentImg" alt="image">
        </figure>
      </div>
      <div class="card-content">
        <div class="media">
          <div class="media-content">
            <p class="title is-4">{{ num }} - {{ upper(name)}}</p> <!--filter-->
            <p class="subtitle is-6">{{ pokemon.type }}</p>
          </div>
        </div>

        <div class="content">
          <button class="button is-medium is-fullwidth is-link" id="buscaBtn" @click="mudarSprite">Mudar Sprite</button><!--mudar pokemon -->
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  created: function(){//pegando conteúdo das props
    axios.get(this.url).then(res =>{
      this.pokemon.type = res.data.types[0].type.name;
      this.pokemon.front = res.data.sprites.front_default; //parte da frente (sprite - var da api)
      this.pokemon.back = res.data.sprites.back_default; //parte de trás
       this.currentImg = this.pokemon.front;
    }).catch(err =>{
      console.log(err)
    })
  },
  data(){
    return{
      isFront: true,
      currentImg: '',
      pokemon:{
         type: '',
         front: '', //tornando props reativas
         back: ''
      }
    }
  },
  props: {
     num: Number,
     name: String,
     url: String
  },
  methods: {

      upper: function(value){
        const newName = value[0].toUpperCase() + value.slice(1); //divide string e remove apenas o priemiro caractere 
        return newName;
      },

      mudarSprite: function(){
    if(this.isFront){
      this.isFront = false;
      this.currentImg = this.pokemon.back;
    }else{
      this.isFront = true;
      this.currentImg = this.pokemon.front;
      }
    }
  }
}
</script>

<style>
   #pokemon {
        margin-top: 2px;
    }
</style>
