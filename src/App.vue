<template>
  <div class="container">
    <h2>Sortable elements</h2>  
    <Sortable v-model="sortableData" />
  </div>  
</template>

<script setup>
import Sortable from './components/Sortable.vue';
import { ref } from 'vue';

const sortableData = ref([{
  id: 1,
  text: 'First row',
  children: [{
    id: 2,
    text: 'Second row',
  }, {
    id: 3,
    text: 'Third row',
    children: [{
      id: 4,
      text: 'Forth row',
    }]
  }]
}, {
  id: 5,
  text: 'Fifth row',
  children: [{
    id: 6,
    text: 'Sixth row',
  }]
}]);
</script>


<style scoped>
.container {
  width: 550px;
  background: #fff;
  border-radius: 8px;
  box-shadow: 0 10px 20px rgba(0, 0, 0, 0.05);
}

.container h2 {
  font-weight: 600;
  font-size: 1.5rem;
  padding: 25px 25px 1rem 25px;
  border-bottom: 1px solid #d4dbe5;
}
</style>
