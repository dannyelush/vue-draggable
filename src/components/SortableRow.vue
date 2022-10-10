<template>
  <div> 
    <div class="row" draggable="true" @dragstart="startDrag($event, data)" @dragend="endDrag($event, data)" :data-drag-id="data.id">
      <div class="sortable-row">
        <div class="header dropzone" @drop="onDrop($event, data)" @dragenter.prevent @dragover.prevent>
          <button type="button" class="drag-btn">
            <span class="material-symbols-outlined">drag_handle</span>
          </button>      
          <span class="details">{{data.id}} {{data.text}}</span>
        </div>
        <button type="button" class="expand" v-if="data.children && data.children.length" @click.prevent="expandToggle(data)">
          <span class="material-symbols-outlined" v-if="data.expanded">expand_less</span>
          <span class="material-symbols-outlined" v-else>expand_more</span>
        </button>
      </div>
      <div v-if="data.children && data.children.length && data.expanded" class="sortable-children">
        <SortableRow v-for="elem in data.children" :key="elem.id" :data="elem" @update="update"/>
      </div>
    </div>
  </div>
</template>

<script setup>
import SortableRow from './SortableRow.vue';

const props = defineProps({
  data: {
    type: Object,
    default: () => ({})
  }
});

const emit = defineEmits(['update']);

function expandToggle(data) {
  data.expanded = !data.expanded;
}

const startDrag = (event, item) => {  
  const id = item.id;
  const elemId = event.target.dataset.dragId;
  if (id && elemId && id.toString() === elemId.toString()) {
    event.target.classList.add("in-drag");
    event.dataTransfer.dropEffect = 'move';
    event.dataTransfer.effectAllowed = 'move';
    event.dataTransfer.setData('itemID', item.id);
  }
}

const endDrag = (event, item) => {
  event.target.classList.remove("in-drag");
}

const onDrop = (event, data) => {
  const itemID = event.dataTransfer.getData('itemID');
  if (itemID.toString() !==  data.id.toString()) {
    emit('update', itemID, data.id);
  }
}

function update(itemID, id) {
  if (itemID.toString() !== id.toString()) {
    emit('update', itemID, id);
  }
}
</script>

<style scoped>
.drag-btn, .expand {
  background: transparent;
  padding: 0;
  border: none;
  outline: none;
  cursor: pointer;
}
.sortable-row {
  display: flex;
  line-height: 30px;
  justify-content: space-between;
  border-bottom: 1px solid #dedede;
  padding: 0 5px;
}

.header {
  display: flex;
  margin: 10px 0;
}

.sortable-children {
  padding-left: 25px;
}

.row {
  padding: 0;
}

.dropzone {
  min-height: 20px;
}
.in-drag {
  background: #f5dbff;
  box-shadow: inset 1px 1px 10px #9b59b6;
}
</style>
