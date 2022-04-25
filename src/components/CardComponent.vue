<template>
<button  class="backBtn"  @click="loadData(prev)"> 
  &lt;
 </button>
  <div class="container">
     <div class="card">
         <div class="img-container">
             <img :src="imgSrc" style="width: 100%;max-height: 100%; object-fit: contain;">
         </div>
         <div class="description">
             <div class="title">{{name}}</div>
             <div class="card-body">
                 <table>
                     <tr><td>Height:</td><td>{{height}} ft</td></tr>
                     <tr><td>Weight:</td><td>{{weight}} Kg</td></tr>
                     <tr><td>Order:</td><td>{{order}}</td></tr>
                     <tr><td>Total Moves:</td><td>{{totalMoves}}</td></tr>
                     <tr><td>Total State:</td><td>{{totalState}}</td></tr>
                     <tr><td>Base Experience:</td><td>{{baseExperience}}</td></tr>
                 </table>
             </div>
         </div>
     </div>
  </div>
  <button class="nextBtn" @click="loadData(next)"> 
  &gt;
 </button>
</template>

<script>
const axios = require('axios')
export default {
async created(){
    await this.loadData()
},
data() {
    return {
        details:[],
        next:'',
        prev:'',
        name:'',
        height:'',
        weight:'',
        order:'',
        totalMoves:'',
        totalState:'',
        baseExperience:'',
        imgSrc:'',
    }
},
methods: {
    formatName(name){
      return name.charAt(0).toUpperCase() + name.slice(1);
    },
    async loadData(dataurl = "https://pokeapi.co/api/v2/pokemon?offset=0&limit=1"){
       const data = await axios.get(dataurl)
    //    console.log(data.data)
       this.next = data.data.next
       this.prev = data.data.previous
       console.log(this.prev)
       const url = data.data.results[0].url
       console.log("ueeeelll",url)
       const res = await axios.get(url)
       this.imgSrc = res.data.sprites.other.dream_world.front_default
       this.name = this.formatName(res.data.name)
       this.height = res.data.height
       this.weight = res.data.weight
       this.order = res.data.order
       this.totalState = res.data.stats.length
       this.totalMoves = res.data.moves.length
       this.baseExperience = res.data.base_experience
    }
},

}
</script>

<style>
html{
height: 100%;  
background-color: #0093E9;
background-image: linear-gradient(160deg, #0093E9 0%, #80D0C7 100%);
}
/*  */
.img-container{
    background-color: #f8f6f6;
    width: 150px;
    height: 150px;
    position: relative; 
    /* left: 50%; */
    
    transform: translateY(-50%);
     box-shadow: 21px 15px 35px 10px rgba(0,0,0,0.16);
    /* top:25px;*/
    margin: auto; 
    border-radius: 50%;
}
table{
    margin: auto;
    text-align: left;
}
.card-body
{
    margin: auto;
    text-align: center;
    font-size: 25px;
}
.title{
    margin: auto;
    font-weight: 600;
    text-align: center;
    font-size: 35px;
    transform: translateY(-100%);
}
.card{
    margin: auto;
    margin-top:100px;
    width: 500px;
    height: 500px;
    background-color: #ffffff;
    border-radius: 15px;
    box-shadow: 21px 15px 35px 10px rgba(0,0,0,0.16);
}

.backBtn{
    font-size: 25px;
    text-align: center;
   position: absolute;
   left: 10px;
   top: 50%;
   width: 40px;
   height: 40px;
   border-radius: 30%;
   border: none;
   background: rgb(255, 255, 255);
box-shadow: 21px 15px 35px -10px rgba(24,131,158,1);
}

.nextBtn{
    text-align: center;
    padding:auto;
    font-size: 25px;
   position: absolute;
   right: 10px;
   top: 50%;
    width: 40px;
   height: 40px;
   border-radius: 30%;
   border: none;
   background: rgb(255, 255, 255);
   box-shadow: 21px 15px 35px -10px rgba(24,131,158,1);
}

.nextBtn:hover,.backBtn:hover{
    background: #0093E9;
    transform: translateY(-10%);
    transition: transform 0.5s;
    color: white;
}

img:hover {
  transform: translateY(-10%);
  transition: transform 0.5s;
}
img:active{
    transform: translateY(0);
  transition: transform 0.5s;
}

</style>