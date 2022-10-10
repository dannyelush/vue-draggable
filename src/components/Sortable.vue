<template>
  <div class="sortable-container">
    <SortableRow v-for="elem in data" :key="elem.id" :data="elem" @update="update"/>
  </div>
</template>


<script setup>
import { ref, watch } from 'vue';
import SortableRow from './SortableRow.vue';

const props = defineProps({
  modelValue: {
    type: Array,
    default: () => ([])
  }
});

const emit = defineEmits(['update:modelValue']);

const data = ref(props.modelValue);

function findElemById(list, id) {
  let result;
  for (let i = 0; i < list.length && !result; i++) {
    const n = list[i];
    if (n.id.toString() === id.toString()) {
      result = n;
      break;
    } else if (n.children && n.children.length) {
      result = findElemById(n.children, id);
    }
  }
  return result;
}

function findParentById(root, id, parent) {
  var node;

  root.some(function (n) {
    if (n.id.toString() === id.toString()) {
      return node = parent;
    }
    if (n.children) {
      return node = findParentById(n.children, id, n);
    }
  });
  return node || null;
}

function update(drag, drop) {
  const parent = findParentById(data.value, drag, null);
  const dragElem = findElemById(data.value, drag);
  const dropElem = findElemById(data.value, drop);
  // Execute only if parent has changed
  if (parent && parent.id !== drop) {  
    if (!dropElem.children) {
      dropElem.children = [];
    }
    if (dragElem && dragElem.id) {
      dropElem.children.push(dragElem);
    }
    dropElem.expanded = true;  

    // Remove from original parent
    if(parent && parent.children) {
      parent.children = parent.children.filter(c => c.id.toString() !== drag.toString())
      if (!parent.children.length) {
        parent.expanded = false;
      }
    } else {
      data.value = data.value.filter(c => c.id.toString() !== drag.toString())
    }
  }
}

watch(data, (newData) => {
  emit('update:modelValue', newData);
}, { deep: true });
</script>

<style scoped>
.sortable-container {
  padding: 25px;
}
</style>
