<template>
  <div>
    <header>
      <nav>
        <ul>
          <li><button @click="currentView = 'todos'">Todos</button></li>
          <li><button @click="currentView = 'posts'">Post</button></li>
        </ul>
      </nav>
    </header>

    <div v-if="currentView === 'todos'">
      <h2>Kegiatan yang Sudah Ditambahkan</h2>
      <ul>
        <li v-for="kegiatan in kegiatanList" :key="kegiatan.id">
          <input type="checkbox" v-model="kegiatan.isDone" />
          <del v-if="kegiatan.isDone">{{ kegiatan.nama }}</del>
          <span v-else>{{ kegiatan.nama }}</span>
          <button @click="deleteKegiatan(kegiatan)">Hapus</button>
        </li>
      </ul>
      <form @submit.prevent="addKegiatan">
        <input type="text" v-model="newKegiatan.nama" placeholder="Tambah Kegiatan Baru" />
        <button type="submit">Tambah</button>
      </form>
    </div>

    <div v-if="currentView === 'posts'">
      <h2>Postingan User</h2>
      <select v-model="selectedUserId" @change="fetchPosts">
        <option value="">Pilih User</option>
        <option v-for="user in users" :key="user.id" :value="user.id">{{ user.name }}</option>
      </select>
      <ul>
        <li v-for="post in posts" :key="post.id">
          <h3>{{ post.title }}</h3>
          <p>{{ post.body }}</p>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      currentView: 'todos',
      kegiatanList: [
        { id: 1, nama: 'Belajar Vue.js', isDone: false },
        { id: 2, nama: 'Meeting dengan Tim', isDone: false }
      ],
      newKegiatan: { nama: '' },
      users: [],
      selectedUserId: '',
      posts: []
    };
  },
  created() {
    this.fetchUsers();
  },
  methods: {
    addKegiatan() {
      this.kegiatanList.push({ id: this.kegiatanList.length + 1, nama: this.newKegiatan.nama, isDone: false });
      this.newKegiatan.nama = '';
    },
    deleteKegiatan(kegiatan) {
      this.kegiatanList = this.kegiatanList.filter(k => k.id !== kegiatan.id);
    },
    fetchUsers() {
      fetch('https://jsonplaceholder.typicode.com/users')
        .then(response => response.json())
        .then(data => {
          this.users = data;
        });
    },
    fetchPosts() {
      if (!this.selectedUserId) return;
      fetch(`https://jsonplaceholder.typicode.com/posts?userId=${this.selectedUserId}`)
        .then(response => response.json())
        .then(data => {
          this.posts = data;
        });
    }
  }
};
</script>

<style>
header {
  background-color: #f8f8f8;
  padding: 10px 20px;
  border-bottom: 1px solid #ccc;
  margin-bottom: 20px;
}

nav ul {
  list-style: none;
  padding: 0;
}

nav ul li {
  display: inline;
  margin-right: 10px;
}

nav ul li button {
  background: none;
  border: none;
  color: #007BFF;
  cursor: pointer;
  padding: 5px 10px;
  font-size: 16px;
}

nav ul li button:hover {
  text-decoration: underline;
}

li {
  list-style: none;
  padding: 10px;
  border-bottom: 1px solid #ccc;
}

li:last-child {
  border-bottom: none;
}

input[type="text"] {
  width: 100%;
  padding: 10px;
  border: 1px solid #ccc;
}

button[type="submit"] {
  background-color: #4CAF50;
  color: #fff;
  padding: 10px 20px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

button[type="submit"]:hover {
  background-color: #3e8e41;
}

select {
  padding: 10px;
  border: 1px solid #ccc;
  margin-bottom: 20px;
}

h3 {
  margin: 0;
}
</style>
