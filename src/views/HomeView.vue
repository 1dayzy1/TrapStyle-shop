<template>


  <div class="container-basket" v-if="this.$store.state.openDrawer">

    <DrawerBlock   :draw="item" :sum="totalSum"
    
    />

  </div>

  


  <div class="modal">

    <CardDesc :it="$store.state.modalbasket" />

  </div>


  <ModalCall />


  

  <!-- <div class="container flex justify-around items-center  w-full h-screen  bg-cover ">

    <HomeLand />

  </div> -->

  


  <div class="container-search">

    <h1 class="text-3xl">Каталог товаров </h1>

    <div class="block-search">

      <select @change="onChange" class="border-1 rounded-md ">


        <option class="option-block" value="name">По названию</option>
        <option class="option-block" value="price">По цене (дешевые)</option>
        <option class="option-block" value="-price">По цене (дорогие)</option>

      </select>

    </div>

  </div>

  <div class="flex flex-wrap">



    <CardBlock v-for="it in items" :key="it.id" :it="it" />





  </div>





  <NotificationBlock />


  <!-- <SliderBlock /> -->







</template>

<script>
import CardBlock from '@/components/CardBlock.vue';
import CardDesc from '@/components/CardDesc.vue';
import DrawerBlock from '@/components/DrawerBlock.vue';
// import HomeLand from '@/components/HomeLand.vue';  
import ModalCall from '@/components/ModalCall.vue';
import NotificationBlock from '@/components/NotificationBlock.vue';
// import SliderBlock from '@/components/SliderBlock.vue';
import axios from 'axios';
import { ref, onMounted, watch } from 'vue';




export default {
  name: 'HomeView',
  components: {
    CardBlock,CardDesc,NotificationBlock,ModalCall,DrawerBlock
  },

 
  
  computed:{
    item(){
      return this.$store.getters.openDraw;
    },

    totalSum(){
      return this.$store.getters.openSum;
    }
  }
,
  
  setup() {
    const items = ref([]); 

    const sortBy = ref('');


    onMounted(async () => {
      try {
        const response = await axios.get('https://0b7366f197401e76.mokky.dev/items');
        items.value = response.data; 

        console.log(items)
      } catch (error) {
        console.error('Error fetching data:', error);
       
      }
    });

    


    


    const onChange = (event) =>{
      sortBy.value = event.target.value;
    }


    watch(sortBy, async () =>{
      try {
        const response = await axios.get('https://0b7366f197401e76.mokky.dev/items?sortBy=' + sortBy.value);
        items.value = response.data; 
        
      } catch (error) {
        console.error('Error fetching data:', error);
       
      }
    })

    return {
      items,
      onChange,
      sortBy,
      

    };
  },



  
  

 
  
}
</script>


<style scoped>


.container{
  background-image: url('../assets/img/bgimage.png');
  width: 100%;
  
  background-size: cover;
  
}




.container-search{
  display: flex;
  justify-content: space-around;
  align-items: center;
  margin-top: 50px;
}





@media (max-width:730px) {
  .container{
    flex-direction: column;
  }
}




/* .modal{
  position: fixed;
  left: 15vw;

  
  
  
  
} */

</style>
