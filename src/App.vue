<template>
  <div>
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
</template>

<script>
export default {
  data() {
    return {
      kegiatanList: [
        { id: 1, nama: 'Belajar Vue.js', isDone: false },
        { id: 2, nama: 'Meeting dengan Tim', isDone: false }
      ],
      newKegiatan: { nama: '' }
    };
  },
  methods: {
    addKegiatan() {
      this.kegiatanList.push({ id: this.kegiatanList.length + 1, nama: this.newKegiatan.nama, isDone: false });
      this.newKegiatan.nama = '';
    },
    deleteKegiatan(kegiatan) {
      this.kegiatanList = this.kegiatanList.filter(k => k.id !== kegiatan.id);
    }
  }
};
</script>
<style>
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
</style>