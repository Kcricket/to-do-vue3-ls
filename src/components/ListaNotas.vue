<template lang="html">
<transition-group name="list-complete">
 <div data-ani="bounceInRight" class="cardDiv" v-for="(item, index) in items" v-bind:key="index" v-show="item.visible" v-bind:class="{cardDivA: item.animationOut}">
   <div class="divCol">
      <div class="divCuerpo">
        <h2 v-bind:class="{bookCard:item.completed}">{{item.book}},  {{item.completed==true? "Done": "To Do"}}</h2>
      </div>
      <div class="divDate" v-bind:style="{backgroundColor:getPrio(item)}">
        <p>{{timeDifference(Date.now(), item.date)}}</p>
      </div>
   </div>
    <div class="btnPrio"> 
      <h3>{{item.priority}}</h3>
    </div>
    <div class="btnPrio">
      <!-- //Reasigna segun el numero y actualiza localstorage -->
      <button @click="changePrio(items, item, 1)">Low</button>
      <button @click="changePrio(items, item, 2)">Mid</button>
      <button @click="changePrio(items, item, 3)">High</button>
    </div>
    <button id="btnItem" @click="deleteItem(index, items)">Delete</button>
    <button id="btnItem" @click="changeCompleted(items, item)">Change completed</button>

 </div>
 <div class="divOptions">
        <div>
            <button @click="ordenaOculta(items, 3)">All</button>
            <button @click="ordenaOculta(items, 1)">Uncompleted</button>
            <button @click="ordenaOculta(items, 2)">Completed</button>
        </div>
        <button @click="clearCompleted(items)">Clear completed</button>
    </div>
  </transition-group>
</template>

<script lang="js">
import 'animate.css';

  export default {
    name: 'ListaNotas',
    props: ['items'],
    mounted () {

    },
    data () {
      return {
      }
    },
    methods: {
      deleteItem(index, items){
          items.splice(index, 1)
          localStorage.items = JSON.stringify(items);
          items[index].animationOut =true
          window.setTimeout(function(){
          items[index].animationOut = false
           }, 3000);
          console.log("inserted in LS")
      },
      clearCompleted(items){
        for(var i=0; i<items.length; i++){
          if(items[i].completed == true){
            items.splice(i, 1)
            i--
            localStorage.items = JSON.stringify(items);
            console.log("inserted in LS")
          }
        }
      },
      timeDifference(current, previous) {
        console.log(previous)
        console.log(current)

          var msPerMinute = 60 * 1000;
          var msPerHour = msPerMinute * 60;
          var msPerDay = msPerHour * 24;
          var msPerMonth = msPerDay * 30;
          var msPerYear = msPerDay * 365;
          
          var elapsed = current - previous;
          
          if (elapsed < msPerMinute) {
              return Math.round(elapsed/1000) + ' seconds ago';   
          }
          
          else if (elapsed < msPerHour) {
              return Math.round(elapsed/msPerMinute) + ' minutes ago';   
          }
          
          else if (elapsed < msPerDay ) {
              return Math.round(elapsed/msPerHour ) + ' hours ago';   
          }

          else if (elapsed < msPerMonth) {
              return 'approximately ' + Math.round(elapsed/msPerDay) + ' days ago';   
          }
          
          else if (elapsed < msPerYear) {
              return 'approximately ' + Math.round(elapsed/msPerMonth) + ' months ago';   
          }
          
          else {
              return 'approximately ' + Math.round(elapsed/msPerYear ) + ' years ago';   
          }
      },
      getPrio(item){
        var prio = ""
        if(item.priority=="low"){
          prio= "#FFA1C9"
        }else if(item.priority=="mid"){
          prio= "#F94892"
        }else if(item.priority=="high"){
          prio= "#E60965"
        }
        return prio
      },  
      changePrio(items, item, prio){
        switch (prio) {
          case 1: item.priority = "low"
          break;
          case 2: item.priority = "mid"
          break;
          case 3: item.priority = "high"
          break;
        
          default:
            break;
        }
        localStorage.items = JSON.stringify(items);
        console.log("inserted in LS")
      },
      changeCompleted(items, item){
        item.completed = !item.completed
        localStorage.items = JSON.stringify(items);
        console.log("inserted in LS")
      },
      //Enseña solo completados/ incompletados
      ordenaOculta(items, num){
        var uncomplete = []
        var complete =[]
        items.forEach(element => {
          element.visible= true
          if(element.completed == true){
            uncomplete.push(element)
          }else{
            complete.push(element)
          }
        });
        if(num == 1){
          uncomplete.forEach(element => {
            element.visible= false
          });
        }else if(num ==2){
          complete.forEach(element => {
            element.visible= false

          });
        }
        else if(num == 3){
          // items.forEach(element => {
          //   var low, mid, high, concat;
          //   if(element.priority == "low"){
          //     low.push(element)
          //   }else if(element.priority == "mid"){
          //     mid.push(element)
          //   }else{
          //     high.push(element)
          //   }
          // });
          // return concat.concat(low, mid, high)
        }
        
      }
    },
      // ordenaPrio(items, num){

      // }
    computed: {
      
    }
}


</script>

<style scoped>
/* //card colors */
button {
  color: rgb(255, 255, 255); font-size: 13px; line-height: 13px; padding: 9px; border-radius: 22px; font-family: Georgia, serif; font-weight: normal; text-decoration: none; font-style: normal; font-variant: normal; text-transform: none; background-image: linear-gradient(45deg, rgb(230, 9, 101) 0%, rgb(249, 72, 146) 53%, rgb(251, 229, 229) 100%); box-shadow: rgba(0, 0, 0, 0.82) 1px 1px 26px 7px; border: 2px none rgb(28, 110, 164); display: inline-block; margin: 5px;}
button:hover {
  background: #E60965; }
button:active {
  background: #FBE5E5; }

.bookCard{
    color:deepskyblue;
    text-decoration: line-through;
    text-decoration-color: red;

}
.divCol{
  display: flex;
  flex-direction: column;
  width: 90%;
  padding-bottom: 20px;
}
.divDate{
    padding: 10px;
}
.divOptions{
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    align-items: center;
    width: 90%;
    padding-left: 20px;
    padding-right:20px;

}

.cardDiv{
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  align-items:center;
  margin: 20px;

  background: #D47AE8;
  background: -moz-linear-gradient(top, #D47AE8 0%, #F4BEEE 50%, #A8ECE7 100%);
  background: -webkit-linear-gradient(top, #D47AE8 0%, #F4BEEE 50%, #A8ECE7 100%);
    -webkit-box-shadow: 2px 2px 48px 22px #E60965; 
    box-shadow: 2px 2px 48px 22px #E60965;

  width: 90%;
  padding-right: 20px;
  padding-left: 20px;
  padding-bottom: 5px;
  padding-top: 10px;
  -webkit-border-radius: 26px 0px 26px 0px;
  border-radius: 26px 0px 26px 0px;


      transition: all 0.8s ease;
  margin-right: 10px;
}
.list-complete{
    width: 100%;
}
.list-complete-enter-from,
.list-complete-leave-to {
  opacity: 0;
  transform: translateY(30px);
}

.list-complete-leave-active {
  position: absolute;
}

.cardDivA{
        animation: infinite;

      display: flex;
  flex-direction: row;
  justify-content: space-between;
  align-items:center;
  margin: 20px;

  background: #D47AE8;
  background: -moz-linear-gradient(top, #D47AE8 0%, #F4BEEE 50%, #A8ECE7 100%);
  background: -webkit-linear-gradient(top, #D47AE8 0%, #F4BEEE 50%, #A8ECE7 100%);
    -webkit-box-shadow: 2px 2px 48px 22px #E60965; 
    box-shadow: 2px 2px 48px 22px #E60965;

  width: 90%;
  padding-right: 20px;
  padding-left: 20px;
  padding-bottom: 5px;
  padding-top: 10px;
  -webkit-border-radius: 26px 0px 26px 0px;
  border-radius: 26px 0px 26px 0px;

}

.divCuerpo{
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  width: 90%;
  flex-wrap: wrap;

}
.btnPrio{
    display: flex;
    flex-direction: column;
    margin-right: 10px;
    margin-left: 20px;

}
h2{
  width: 90%;
  word-wrap: break-word;
}

/* Here goes 3 types of divs */
</style>
