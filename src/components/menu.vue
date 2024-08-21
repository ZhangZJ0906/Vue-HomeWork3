<template>


<div class="col-md-4">
          <div class="list-group" v-for="item in item" :key="item.id" @click.prevent="addItem(item)">
            <a href="#" class="list-group-item list-group-item-action">
              <div class="d-flex w-100 justify-content-between">
                <h5 class="mb-1">{{ item.name }}</h5>
                <small>{{ item.price }}</small>
              </div>
              <p class="mb-1">{{ item.description }}</p>
            </a>
          </div>
        </div>
</template>

<script setup>
const props=defineProps({
  item:Array
});
const emits=defineEmits(['add-item']);

const addItem=(item)=>{
  emits('add-item',item);
}


</script>