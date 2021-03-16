<template lang='html'>
    <div class=''>

        

    <div class='card-container'>
        

        
        <div v-for='character in characters' class='card' >
            <h3>{{character.name}} - {{character.index}}</h3>
            <router-link class='link' :to="{name: 'character', params: {id : character.id} }" >
                <button type='button' class='btn btn-view'>View</button>
               
            </router-link>
            <button class="btn btn-danger" @click='borrar(index )'>
                <img src="../assets/trash.png"/>
            </button>
        </div>

      
    </div>

    </div>
</template>

<script>

import {public_key, secret_key} from '../marvel.js';
import axios from 'axios';
import '../assets/style/characters.css'
export default {
    name : 'Characteres',
    data(){
        return{
            characters: [],
            url: '',
        size: 'standard_large.jpg'
        }
    },

mounted(){
    this.getCharacters()

},

  methods: {
    getCharacters: function(){

      axios.get(`https://gateway.marvel.com/v1/public/characters?apikey=${public_key}&hash=${secret_key}`)
        .then ((result) => {
            
            result.data.data.results.forEach((item) => {

                console.log(item)
                this.characters.push(item)
                this.url = `${item.thumbnail.path}/${this.size}`
                    console.log(this.url)
            })

            /*


Por ejemplo, un usuario con una clave pública de "1234" 
y una clave privada de "abcd" podría construir una llamada válida de la siguiente manera: 
http://gateway.marvel.com/v1/public/comics?ts=1&apikey=1234&hash=ffd275c5130566a2916217b101f26150 
(el valor hash es el resumen md5 de 1abcd1234)


            */
        })
        .catch((error) =>{
            console.log(error)
        })
      
    },
    borrar (index){
            this.characters.splice(index, 1)
        }
  }
}
</script>

<style lang="css">

</style>