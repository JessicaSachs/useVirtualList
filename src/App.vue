<template>
    <div class="w-800px pt-24 mx-auto space-y-4 text-left">    
    <h2 class="text-3xl pb-12 text-center">useVirtualList Demo</h2>
    <p>Use the input to filter down the search list below. Currently, the list is not reactive :-(</p>
    <input class="ring-1" v-model="search"/> {{ search }}

    <h3 class="text-xl">Big List</h3>
    <div>
      <p>Number of total Items: {{ bigList.length }}</p>
      <p>Number of items from useVirtualList: {{ list.length }}</p>
      <p>Number of items from filteredList: {{ filteredList.length }}</p>
    </div>

    <div v-bind="containerProps" class="max-h-500px ring-1">
      <div v-bind="wrapperProps" class="children:h-80px divide-y-12">
        <div v-for="row in list" :key="row.data.id">
          <b>{{ row.index + 1 }}</b>
          {{ row.data.name }}
        </div>
      </div>
    </div>
    </div>
</template>

<script setup lang="ts">
import { ref, computed } from 'vue'
import faker from 'faker'
import { useVirtualList } from '@vueuse/core'

const search = ref('')

const bigList = Array.from(Array(100).keys()).map(a => ({
  ...faker.helpers.createCard(),
  id: faker.datatype.uuid()
}))

const filteredList = computed(() => {
  return bigList.filter(i => {
    return i.name.toLocaleLowerCase().indexOf(search.value.toLocaleLowerCase()) > -1
  })
})

// Ideally, I could pass something reactive into `useVirtualList`
const { list, wrapperProps, containerProps } = useVirtualList(filteredList.value, { itemHeight: 80 })
</script>
