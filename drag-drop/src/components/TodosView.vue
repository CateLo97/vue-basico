<script setup>
import { reactive } from 'vue';
import InputNew from './inputNew.vue';

const boards = reactive([
  {
    id: crypto.randomUUID(),
    name: 'Tablero 1',
    items: [
      { id: crypto.randomUUID(), title: 'Feature de archivos' },
      { id: crypto.randomUUID(), title: 'Resolver bugs' }
    ]
  },
  {
    id: crypto.randomUUID(),
    name: 'Tablero 2',
    items: [
      { id: crypto.randomUUID(), title: 'Mandar reporte' },
      { id: crypto.randomUUID(), title: 'Code review' }
    ]
  }
]);

function handleNewItem(text, board) {
    board.items.push({
        id: crypto.randomUUID(),
        title: text,
    });

}

function handleNewBoard() {
    const name = prompt('Nombre del nuevo tablero');
    if (name) {
        boards.push({
            id: crypto.randomUUID(),
            name,
            items: []
        });
    }
}

function startDrag(event, board, item) {
    event.dataTransfer.setData('text/plain', JSON.stringify({boardId:board.id, itemId:item.id}));
    event.dataTransfer.effectAllowed = 'move';
}

function onDrop(event, dest){
    const { boardId, itemId } = JSON.parse(event.dataTransfer.getData('text/plain'));
    const sourceBoard = boards.find(board => board.id === boardId);
    const item = sourceBoard.items.find(item => item.id === itemId);
    dest.items.push(item);
    sourceBoard.items = sourceBoard.items.filter(i => i.id !== itemId);
}


</script>

<template>
  <nav>
    <ul>
      <li><a href="#" @click.prevent="handleNewBoard">Create board</a></li>
    </ul>
  </nav>

  <div class="boards-container">
    <div class="boards">
      <div 
      class="board" 
      @drop="onDrop($event, board)" 
      @dragover.prevent 
      @dragenter.prevent 
      v-for="board in boards" 
      :key="board.id">
        <h2>{{ board.name }}</h2>
        <InputNew @on-new-item="(text) => handleNewItem(text, board)" />
        <div class="items">
          <div 
          class="item" 
          draggable="true" 
          @dragstart="startDrag($event, board, item)" 
          v-for="item in board.items" 
          :key="item.id" >
            {{ item.title }}
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>

nav{
    background-color: #514141;
    padding: 10px;
    border-radius: 5px;
    margin-bottom: 10px;
}

nav ul{
    list-style: none;
    padding: 0;
    margin: 0;
    display: flex;
    gap: 10px;
}

nav ul li{
    padding: 10px;
    border-radius: 5px;
    cursor: pointer;
}


.boards{
    display: flex;
    gap: 10px;
}
.board{
    background-color: #514141;
    padding: 10px;
    border-radius: 5px;

}

.items{
    display: flex;
    flex-direction: column;
    gap: 10px;
    background-color: #514141;
    padding: 10px;
    border-radius: 5px;
}
.item{
    background-color: #817474;
    padding: 10px;
    margin: 5px 0;
    border-radius: 5px;
    cursor: move;
}
</style>