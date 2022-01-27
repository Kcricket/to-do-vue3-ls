<template>
<div id="counter">
    <div class="inputDiv">
        <input type="text" class="input" placeholder="holawenas" v-model="nuevonombre" v-if="empty">
        <button id="btn" v-on:click="persistStorage">Incrementa</button>
    </div>
    <div class="divFormChecks">
        <div>
            <label for="check">Read</label>
            <input type="checkbox" name="" id="check" v-model="check">
        </div>
        <div class="divChecks">
            <label class="container">Low
            <input type="radio" name="radio" value="low" v-model="check1">
            <span class="checkmark"></span>
            </label>
            <label class="container">Mid
            <input type="radio" name="radio" value="mid" v-model="check1">
            <span class="checkmark"></span>
            </label>
            <label class="container">High
            <input type="radio" name="radio" value="high" v-model="check1">
            <span class="checkmark"></span>
            </label>
        </div>
    </div>
    <div class="divForm">
        <!-- <p class="txt"> {{nuevonombre}} </p> -->
        <div class="divInfoItems">
            <h3>Todos: {{ counter }}</h3>
            <p>Done Todo: {{ getReadedBooks.readedBooks }}</p> 
            <p>Undone Todo: {{ getReadedBooks.unread }}</p>
        </div>

        <div class="searchInp">
            <h3>Search notes here: </h3>
            <input id="searchInput" @keyup="filterNotes" type="text" class="input" placeholder="search a note" v-model="this.searchInput"> 
            <button @click="filterNotes">Filter</button>
        </div>
    </div>
  <div class="notasNOptions">
        <div class="notasContainer">
            <ListaNotas :items="this.items"/>
        </div>
  </div>
</div>      
</template>

<script>
import ListaNotas from './ListaNotas.vue'
export default {
  name: 'AddBookForm',
  components:{
      ListaNotas
  },
  props: {
    msg: String,
    color:String
  },

  data() {
    return {
        string: "Yea",
        nuevonombre:"",
        searchInput:"",
        check: false,
        check1:"",
        items : [
            {
                book: "El prinsipitillo",
                completed:true,
                date: Date.now(),
                priority: "low",
                visible: true,
                animationOut: false,
                animationIn: false
            },
        ],
        counter: 0,
        empty: true
    }
  },

  methods:{
      incrementa(){
          this.insertName();
      },
      insertName(){
          if(this.nuevonombre ===""){
              alert("Vacio bro")
          }else{
              this.items.push(
                  {
                      book: this.nuevonombre, 
                      completed: this.check, 
                      date: Date.now(), 
                      priority: this.check1,
                      visible: true,
                      prioNum: 1
                    })
                    //Meterlos en localStore
              this.nuevonombre = "" 
              this.counter = this.items.length; 
          }
            this.items.sort(function(a,b){return a.prioNum-b.prioNum})
          console.log(this.items)
      },
      checkPrio(){
          this.items = JSON.parse(localStorage.items);
            this.items.forEach(element => {
                if(element.priority== "low"){
                    element.prioNum = 3
                }else if(element.priority =="mid"){
                    element.prioNum = 2
                }else if(element.priority == "high"){
                    element.prioNum = 1
                }
            });
      },
      persistStorage(){
        this.insertName()
        this.items.sort((function(a,b){return a.prioNum-b.prioNum}))
        localStorage.items = JSON.stringify(this.items);
        console.log("inserted in LS")
      },
      filterNotes(){
          let key= this.searchInput
          var result=[]
          this.items.forEach(element => {
              element.visible = true
              if(element.book.includes(key)){
                  result.push(element)
              }else{
                  element.visible = false
              }
          });
      }

  },
  computed:{
      getReadedBooks(){
          var readedBooks = 0
          var unread= 0
          this.items.forEach(element => {
              if(element.completed == true){
                  readedBooks++
              }else{
                  unread++
              }
          });
          return {readedBooks, unread}
      }
  },
      mounted() {
        if (localStorage.items) {
            //Esta linea hace que todo se actualice con el localhost cada vez que recargas
            //Hace que no se pierda la info 
            this.checkPrio()
            this.items.sort(function(a,b){return a.prioNum-b.prioNum})
            this.counter = this.items.length

        }else{
            //this.items.sort((a, b) => a.priority.localeCompare(b.priority))
            localStorage.setItem("items", JSON.stringify(this.items))
        }
    },
    watch:{
        items: function(){
            //localStorage.items = JSON.stringify(this.items);
            console.log("Items modified")
        }
    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
:root{
    --color1:#FBE5E5;
    --color2: #FFA1C9;
    --color3: #F94892;
    --color4: #E60965;
}
button{
    color: rgb(255, 255, 255); font-size: 17px; line-height: 17px; padding: 12px; border-radius: 21px; font-family: Georgia, serif; font-weight: normal; text-decoration: none; font-style: normal; font-variant: normal; text-transform: none; background-image: linear-gradient(rgb(230, 9, 101) 0%, rgb(255, 161, 201) 53%, rgb(230, 9, 101) 100%); box-shadow: rgba(0, 0, 0, 0.82) 1px 1px 26px 7px; border: 2px none rgb(28, 110, 164); display: inline-block;}
button:hover {
    background: #E60965; }
button:active {
    background: #E60965; }

input{
    width: 80%;
  padding: 12px 20px;
  margin: 8px 0;
  box-sizing: border-box;
  border: none;
  background-color: #E60965;
  color: white;
}

.inputDiv{
    width: 100%;
    display: flex;
    justify-content: center;
    align-items: center;
}
.divInfoItems{
    display: flex;
    flex-direction: column;
    align-items: center;

    background-color: #F94892;
    -webkit-box-shadow: 2px 2px 48px 22px #E60965; 
    box-shadow: 2px 2px 48px 22px #E60965;
    width: 70%;
    padding: 10px;
    margin : 20px;
    margin-top: 50px
}
.searchInp{
    width: 70%;
    justify-content: space-between;
    margin: 10px
}
.divForm{
    width: 100%;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    align-items: center;
    flex-wrap: wrap;

}
.divFormChecks{
        display: flex;
        justify-content: space-between;
        align-items: center;
        width: 50%;
}
.divChecks{

    display: flex;
    justify-content: center;
    align-items: center;
    width: 90%;
}
.notasNOptions{
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center ;
    width: 100%;
}
.notasContainer{
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    /* background-color: var(--color2); */
    width: 100%;

}
#counter{
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    background-color:#FBE5E5; 
    width: 70%;
}
</style>
