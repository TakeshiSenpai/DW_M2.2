<template>
  <br><br><br><br>
  <v-container>
    <v-data-table :headers="headers" :items="todos" class="elevation-1">
      <template v-slot:item.actions="{ item }">
        <v-icon small class="mr-2" @click="editItem(item)">
          mdi-pencil
        </v-icon>
        <v-icon small @click="deleteItem(item)">
          mdi-delete
        </v-icon>
      </template>
    </v-data-table>

  </v-container>
  <v-dialog v-model="dialog" persistent max-width="600px">
    <v-card>
      <v-card-title>
        <span class="text-h5">{{ formTitle }}</span>
      </v-card-title>
      <v-card-text>
        <v-container>
          <v-row>
            <v-col cols="12">
              <v-text-field v-model="editedItem.title" label="Title"></v-text-field>
            </v-col>
            <v-col cols="12">
              <v-checkbox v-model="editedItem.completed" label="Completed"></v-checkbox>
            </v-col>
          </v-row>
        </v-container>
      </v-card-text>
      <v-card-actions>
        <v-spacer></v-spacer>
        <v-btn color="blue darken-1" text @click="close">Cancel</v-btn>
        <v-btn color="blue darken-1" text @click="save">Save</v-btn>
      </v-card-actions>
    </v-card>
  </v-dialog>

  <v-btn @click="editItem({})">Add New Todo</v-btn>

</template>

<script>
import axios from 'axios';

export default {
  data: () => ({
    todos: [], // Aquí se almacenarán los datos de la API
    headers: [ // Define los encabezados de la tabla
      {title: 'Autor', key: 'id'},
      {title: 'Titulo', key: 'title'},
      {title: 'Completado', key: 'completed'},
      {title: 'Acciones', value: 'actions', sortable: false},

    ],
    dialog: false,
    editedIndex: -1,
    editedItem: {
      id: '',
      title: '',
      completed: false,
    },
    defaultItem: {
      id: '',
      title: '',
      completed: false,
    },
    formTitle: '',
  }),
  created() {
    this.fetchData();
  },
  methods: {
    async fetchData() {
      try {
        const response = await axios.get('https://jsonplaceholder.typicode.com/todos');
        this.todos = response.data;
      } catch (error) {
        console.error('There was an error fetching the data:', error);
      }
    },
    editItem(item) {
      this.editedIndex = this.todos.indexOf(item);
      this.editedItem = Object.assign({}, item);
      this.dialog = true;
    },
    deleteItem(item) {
      const index = this.todos.indexOf(item);
      confirm('Are you sure you want to delete this item?') && this.todos.splice(index, 1);
    },
    close() {
      this.dialog = false;
      setTimeout(() => {
        this.editedItem = Object.assign({}, this.defaultItem);
        this.editedIndex = -1;
      }, 300);
    },
    save() {
      if (this.editedIndex > -1) {
        Object.assign(this.todos[this.editedIndex], this.editedItem);
      } else {
        this.todos.push(this.editedItem);
      }
      this.close();
    },
  },
};

</script>
