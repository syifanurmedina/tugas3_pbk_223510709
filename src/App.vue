<template>
  <div class="container">
    <h1 class="title">My To Do List</h1>
    <div class="add-list">
      <input type="text" v-model="newList" placeholder="Add Here">
      <input type="text" v-model="newActivity" placeholder="Activity">
      <button @click="addList">Add</button>
    </div>

    <table class="list-table">
      <thead>
        <tr>
          <th></th>
          <th>List</th>
          <th>Activity</th>
          <th>Reactions</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(list, index) in filteredList" :key="index" :class="{ 'completed': list.completed }">
          <td><input type="checkbox" v-model="list.completed" class="checkbox"></td>
          <td>
            <div v-if="!list.editing" class="list-text">{{ list.title }}</div>
            <div v-else class="edit-mode">
              <input v-model="list.title" class="edit-input">
            </div>
          </td>
          <td>
            <div v-if="!list.editing" class="list-text">{{ list.activity }}</div>
            <div v-else class="edit-mode">
              <input v-model="list.activity" class="edit-input">
            </div>
          </td>
          <td>
            <div v-if="!list.editing" class="button-group">
              <button class="edit-btn" @click="editList(index)">Edit</button>
              <button class="delete-btn" @click="deleteList(index)">Delete</button>
            </div>
            <div v-else class="button-group">
              <button class="update-btn" @click="updateList(index)">Update</button>
              <button class="cancel-btn" @click="cancelList(index)">Cancel</button>
            </div>
          </td>
        </tr>
      </tbody>
    </table>

    <div class="filters">
      <input type="text" v-model="searchQuery" placeholder="Search List">
    </div>
    
    <button class="filter-btn" @click="toggleShowIncomplete">
      {{ showIncomplete ? 'Show All List' : 'Show Incomplete List ' }}
    </button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      list: [],
      newList: '',
      newActivity: '',
      showIncomplete: true,
      selectedActivity: '',
      searchQuery: ''
    };
  },
  computed: {
    filteredList() {
      let filtered = this.list.filter(list => {
        if (this.showIncomplete && list.completed) return false;
        if (this.selectedActivity && list.activity !== this.selectedActivity) return false;
        if (this.searchQuery && !list.title.toLowerCase().includes(this.searchQuery.toLowerCase())) return false;
        return true;
      });
      return filtered;
    },
    activity() {
      let activity = new Set(this.list.map(list => list.activity));
      return Array.from(activity);
    }
  },
  methods: {
    addList() {
      if (this.newList.trim() !== '') {
        this.list.push({ title: this.newList, completed: false, activity: this.newActivity || 'Unactivity' });
        this.newList = '';
        this.newActivity = '';
      }
    },
    cancelList(index) {
      this.list[index].editing = false;
    },
    editList(index) {
      this.list[index].editing = true;
    },
    updateList(index) {
      this.List[index].editing = false;
    },
    deleteList(index) {
      this.list.splice(index, 1);
    },
    toggleShowIncomplete() {
      this.showIncomplete = !this.showIncomplete;
    },
    filterByActivity() {
      // Implement if needed
    }
  }
};
</script>

<style scoped>
.container {
  max-width: 680px;
  margin: 0 auto;
  padding: 20px;
  border: 1px solid #000000;
  border-radius: 5px;
  background-color: #f0c1ca; /* Warna latar belakang */
}

.title {
  text-align: center;
  color: #241414;
}

.add-list {
  margin-bottom: 8px;
}

.add-list input[type="text"] {
  margin-right: 8px;
  padding: 8px;
  width: 170px;
  border: 1px solid #000000;
  border-radius: 5px;
}

.add-list button {
  padding: 8px 15px;
  background-color: #9ba79c;
  color: #000000;
  border: 1px solid #000000; /* Penambahan border di dalam */
  cursor: pointer;
  border-radius: 5px;
}

.add-list button:hover {
  background-color: #4c587d;
}

.list-table {
  width: 100%;
  border-collapse: collapse;
  border: 1px solid #fcf7ec;
  border-radius: 5px;
}

.list-table th,
.list-table td {
  border: 1px solid #fcf7ec;
  padding: 10px;
}

.list-table th {
  background-color: #9ba79c;
  color: #000000;
  text-align: left;
}

.list-table tr:nth-child(even) {
  background-color: #f2f2f2;
}

.checkbox {
  margin: 0;
}

.edit-input {
  width: 100%;
}

.button-group {
  display: flex;
  justify-content: flex-start;
  gap: 10px;
}

.button-group button {
  padding: 8px 15px;
  background-color: transparent;
  color: #000000;
  border: 1px solid #9ba79c; /* Penambahan border di dalam */
  cursor: pointer;
  border-radius: 5px;
}

.button-group button:hover {
  background-color: #4c587d;
  color: #000000;
}

.filter-btn {
  padding: 10px 15px;
  background-color: #9ba79c;
  color: #000000;
  border: 1px solid #9ba79c; /* Penambahan border di dalam */
  cursor: pointer;
  border-radius: 5px;
}

.filter-btn:hover {
  background-color: #4c587d;
}
</style>