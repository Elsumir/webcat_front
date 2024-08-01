<template>
  <div>
    <h1>Список персонала</h1>
    <input v-model="newItem.name" placeholder="Имя" />
    <input v-model="newItem.position" placeholder="Должность" />
    <input v-model="newItem.experience" type="number" placeholder="Стаж" />
    <button @click="addItem">Добавить</button>
    <ul>
      <ToDoItem
        v-for="item in items"
        :key="item.ID"
        :item="item"
        @delete="deleteItem(item.ID)"
        @edit="editItem($event)"
      />
    </ul>
  </div>
</template>

<script>
import ToDoItem from './ToDoItem.vue';
import axios from 'axios';

export default {
  components: {
    ToDoItem,
  },
  data() {
    return {
      newItem: {
        name: '',
        position: '',
        experience: 0,
      },
      items: [],
      id: 0,
      apiUrl:
        'http://serikavj.beget.tech/rest/1/afy8tjyxkpe1jy4u/employee.list.',
    };
  },
  methods: {
    async addItem() {
      if (
        this.newItem.name.trim() &&
        this.newItem.position.trim() &&
        this.newItem.experience >= 0
      ) {
        const name = this.newItem.name;
        const position = this.newItem.position;
        const experience = this.newItem.experience;
        const data = { name, position, experience };

        await axios.post(this.apiUrl + 'addEmployee', {
          data,
        });

        this.fetchGetEmployees();

        this.newItem.name = '';
        this.newItem.position = '';
        this.newItem.experience = 0;
      } else {
        alert('заполните все поня');
      }
    },
    async deleteItem(id) {
      await axios.post(this.apiUrl + 'deleteEmployee', {
        id,
      });
      this.fetchGetEmployees();
    },
    async editItem(data) {
      await axios.post(this.apiUrl + 'updateEmployee', {
        data,
      });
      this.fetchGetEmployees();
    },
    async fetchGetEmployees() {
      const res = await axios.get(this.apiUrl + 'getEmployees');
      this.items = res.data.result;
    },
  },
  mounted() {
    this.fetchGetEmployees();
  },
};
</script>

<style scoped>
ul {
  list-style-type: none;
}

li {
  display: flex;
  margin-bottom: 10px;
}
</style>
