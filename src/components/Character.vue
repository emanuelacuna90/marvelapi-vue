<template lang="html">
<div class='container-card'>
    <div class='container-heroe'>
        <h2 v-for="char in character">{{char.name}}</h2>
            
                <p class='description' v-for="char in character">
                    {{char.description}}
                </p>
                <img :src="url"/>
           
    </div>
</div>
</template>

<script>
import axios from 'axios'
import {public_key, secret_key} from '../marvel.js';
import '../assets/style/character.css'

export default {

name : 'Character',

data(){
    return {
        character: [],
        url: '',
        size: 'portrait_uncanny.jpg'
    }
},

mounted(){
    this.getCharactere()

},

methods:{
    getCharactere : function (){

        var characterId = this.$route.params.id 
        
        axios.get(`http://gateway.marvel.com/v1/public/characters/${characterId}?apikey=${public_key}&hash=${secret_key}`)
            .then ((result) => {
                console.log(result)

                result.data.data.results.forEach((item) => {
                    this.character.push(item)

                    this.url = `${item.thumbnail.path}/${this.size}`
                    console.log(this.url)
                })
            })
            .catch ((error) =>{
                console.log(error)
            })
  
  }
}

}
</script>
