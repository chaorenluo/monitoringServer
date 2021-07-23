<template>
  <div style="padding:20px">
    <a-table :columns="columns" :data-source="errDataRef" bordered>
      <template #species="{ record }">
        {{record.category}}
      </template>
      <template #type="{record}">
        {{record.type}}
      </template>
      <template #tags="{ text: tags }">
      <span>
        <a-tag
            v-for="tag in tags"
            :key="tag"
            :color="tag === 'loser' ? 'volcano' : tag.length > 5 ? 'geekblue' : 'green'"
        >
          {{ tag.toUpperCase() }}
        </a-tag>
      </span>
      </template>
      <template #action="{ record }">
      <span>
        <a>Invite 一 {{ record.name }}</a>
        <a-divider type="vertical" />
        <a>Delete</a>
        <a-divider type="vertical" />
        <a class="ant-dropdown-link">
          More actions
          <down-outlined />
        </a>
      </span>
      </template>
    </a-table>
  </div>
</template>

<script lang="ts">
import { defineComponent,onMounted,ref } from 'vue'
import HelloWorld from './components/HelloWorld.vue'
import axios from 'axios'

export default defineComponent({
  name: 'App',
  components: {
    HelloWorld
  },
  setup(){
    const errDataRef = ref([])
    const columns = [
      {
        title: '种类',
        dataIndex: 'species',
        width:"10%",
        slots: {  customRender: 'species' },
      },
      {
        title: '类型',
        dataIndex: 'type',
        key: 'type',
        width:"10%",
        slots: {  customRender: 'type' },
      },
      {
        title: '描述',
        dataIndex: 'age',
        key: 'age',
      },
      {
        title: '等级',
        dataIndex: 'level',
        width:"10%",
        key: 'level',
      },{
        title: '时间',
        dataIndex: 'time',
        width:"10%",
        key: 'time',
      }
    ]
    onMounted(()=>{
      axios.get('http://localhost:3000/api/collectively/list').then(res=>{

        errDataRef.value = res.data.data;
      })
    })
    return{
      columns,
      errDataRef,
    }
  }
})
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
