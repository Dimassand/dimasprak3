<script setup lang="ts">
import { ref, computed } from 'vue'

const kegiatanList = ref([
  { id: 1, nama: 'Learn Vue.js', selesai: false },
  { id: 2, nama: 'Build a to-do app', selesai: true }
])
const kegiatanBaru = ref('')
const filterSelesai = ref(false)

const filteredKegiatan = computed(() => {
  if (filterSelesai.value) {
    return kegiatanList.value.filter(kegiatan => !kegiatan.selesai)
  } else {
    return kegiatanList.value
  }
})

const tambahKegiatan = () => {
  if (kegiatanBaru.value.trim() !== '') {
    kegiatanList.value.push({ id: Date.now(), nama: kegiatanBaru.value.trim(), selesai: false })
    kegiatanBaru.value = ''
  }
}

const batalkanKegiatan = (id: number) => {
  kegiatanList.value = kegiatanList.value.filter(kegiatan => kegiatan.id !== id)
}

const toggleSelesai = (kegiatan: { id: number, nama: string, selesai: boolean }) => {
  kegiatan.selesai = !kegiatan.selesai
}
</script>

<template>
  <div id="app">
    <h1>UCY & DIMAS CELL - To-Do List</h1>
    
    <input 
      v-model="kegiatanBaru" 
      @keyup.enter="tambahKegiatan" 
      placeholder="Tambahkan nama kegiatan" 
      class="input-text"
    />
    
    <main>
      <input 
        type="checkbox" 
        v-model="filterSelesai" 
        id="filterCheckbox" 
      />
      <label for="filterCheckbox">Tampilkan yang masih ada</label>
      
      <ul>
        <li v-for="kegiatan in filteredKegiatan" :key="kegiatan.id">
          <input 
            type="checkbox" 
            :checked="kegiatan.selesai" 
            @change="toggleSelesai(kegiatan)" 
            class="checkbox"
          />
          <span :class="{ 'selesai': kegiatan.selesai }">{{ kegiatan.nama }}</span>
          <button 
            @click="batalkanKegiatan(kegiatan.id)" 
            v-if="!kegiatan.selesai" 
            class="button"
          >
            Hapus
          </button>
        </li>
      </ul>
    </main>
  </div>
</template>

<style scoped>
#app {
  font-family: Arial, sans-serif;
  text-align: center;
  margin-top: 20px;
  background-color: #f9f9f9;
  padding: 20px;
  border-radius: 10px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
}

h1 {
  margin: 0 0 20px;
  font-size: 24px;
  color: #333;
}

.input-text {
  width: 70%;
  margin-right: 10px;
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 5px;
}

main {
  margin-top: 20px;
}

ul {
  list-style: none;
  padding: 0;
}

li {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 10px 0;
  border-bottom: 1px solid #ddd;
}

span.selesai {
  text-decoration: line-through;
  color: #aaa;
}

.checkbox {
  margin-right: 10px;
}

.button {
  background-color: #ff4d4d;
  color: white;
  border: none;
  padding: 5px 10px;
  border-radius: 5px;
  cursor: pointer;
}

.button:hover {
  background-color: #ff1a1a;
}
</style>
