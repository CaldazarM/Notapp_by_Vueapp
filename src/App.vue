<script setup>
import {ref,onMounted,computed,watch} from "vue";
const showModel= ref(false)
const newNote=ref('')

const notes=ref([])
const errorMassage=ref(""); //اگه داخل ایف خالی باشه به معنی فالس در نظر گرفته میشه
function getRandomColor(){
  return "hsl("+Math.random()*360 +",100%,75%)";
  
}
const DeletNote=(note)=>{
   
	notes.value = notes.value.filter((t) => t !== note)


}
const addNote= ()=>{
 notes.value.push ({
  text : newNote.value ,
  id:Math.floor(Math.random()*1000000),
  date: new Date().getTime(),
  backgroundColor:getRandomColor(),

}) 

 showModel.value=false,
 newNote.value= "",
 errorMassage.value=" "
  
}
watch(notes,newval =>{
  localStorage.setItem('todos',JSON.stringify(newval))
},{deep: true})//حاجی این دیپ خیلی مهمه
onMounted(()=>{//این و استفاده کردم از این لحاظ که تو رندر سرور ام استفاده میشه
  notes.value=JSON.parse(localStorage.getItem('todos')) || [];
})

/*const savednotes= localStorage.getItem('todos')
if(savednotes){
  notes.value
}*/

</script>
<template>
  <main>
    <div v-if="showModel" class="overlay">
      <div class="modal">
       <textarea v-model.trim="newNote" name="note" id="note" cols="30" rows="10"></textarea> 
       <p v-if="errorMassage">{{ errorMassage }}</p>
       <button @click="addNote" >Add Note</button>
       <button class="close" @click="showModel=false">Close</button>
      </div> </div>
    <div class="container">
      <header>
        <h1>Notes </h1>
      <button @click="showModel = true">+</button>
    </header>
    <div class="cards-container">
      <div  v-for="note in notes " :key="note.id"  class="card" :style="{backgroundColor:note.backgroundColor}"> 
        <!--حاجیییییییی ارور *فور* حل شد هاها-->
        <p  class="main-text"> {{ note.text }}  </p>
          <p class="data"> {{ note.date}} </p>
          <button class="delet" @click="DeletNote(note)">delet</button>

      </div>
 
      
    </div>
    </div>
  </main>
</template>

<style scoped>
main{
  height: 100vh;
  width: 100vw;

}
.container{
  max-width: 1000px;
  margin:  0 auto;
  padding: 10ppx;
}
header{
  display: flex;
  justify-content: space-between;
  align-items: center;
}
h1{
  font-weight: bold;
  margin-bottom: 25px;
  font-size: 70px;
}
header button{
  border: non;
  padding: 10px;
  width: 50px;
  height: 50px;
  cursor: pointer;
  background-color: rgb(21, 20,20);
  border-radius:100px ;
  color: white;
  font-size: 20px;
}
.card{
  width: 225px;
  height: 225px;
  background-color: rgb(237, 182, 44);
  padding: 10px;
  border-radius: 15px;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  margin-right: 20px;
  margin-bottom: 20px;

}
.data{
  font-size: 12.5px;
  font-weight: bold;
  position: relative;
  top:80px
}
.cards-container{
display: flex;
flex-wrap: wrap;
}
.overlay{
position: fixed;
width: 100%;
height: 100%;
background-color: rgba(0,0, 0, 0.77);
z-index: 10;
display: flex;
align-items: center;
justify-content: center;
}
.modal{
  width: 750px;
  background-color: white;
  border-radius:10px ;
  padding: 30px;
  position: relative;
  display: flex;
  flex-direction: column;

}
.modal button{
  padding: 10px 20px;
  font-size: 20px;
  background-color: blueviolet;
  border: none;
  color: white;
  cursor: pointer;
  margin-top:15px ;
}
.modal .close{
  background-color: brown;
}
.modal p{
  color: brown;
}
.delet{
  font-weight: bold;
 height: 30px;
 width: 50px;
 
 background-color:#FAF3F0;
 text-align: center;
 border-radius: 15px;
 position: relative;;
margin: 0;
border-color:#FAF3F0;
top:-8px;
left: 170px;
box-shadow: 0;

}
</style>