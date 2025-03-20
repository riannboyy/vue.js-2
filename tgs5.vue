<script>
import { ref, computed, watch, onMounted } from "vue";

export default {
  name: "Vue3FeaturesComponent",
  setup() {
    const items = ref([
      "Belajar Vue",
      "Membuat Aplikasi",
      "Menggunakan Lifecycle Hooks",
    ]);
    const newItem = ref("");
    const itemRef = ref(null);
    const lastAdded = ref("");

    const addItem = () => {
      if (newItem.value.trim()) {
        items.value.push(newItem.value.trim());
        lastAdded.value = newItem.value.trim();
        newItem.value = "";
      }
    };

    const removeItem = (index) => {
      items.value.splice(index, 1);
    };

    const sortItems = () => {
      items.value.sort((a, b) => a.localeCompare(b));
    };

    const itemCount = computed(() => items.value.length);
    const filteredItems = computed(() => items.value.filter((item) => item));
    const lastItemLength = computed(() => lastAdded.value.length);

    watch(items, (newVal, oldVal) => {
      console.log("Daftar item berubah:", oldVal, "→", newVal);
    });

    onMounted(() => {
      console.log("Komponen telah dimuat");
      console.log("Referensi elemen pertama:", itemRef.value);
    });

    return {
      items,
      newItem,
      addItem,
      removeItem,
      sortItems,
      itemCount,
      filteredItems,
      lastAdded,
      lastItemLength,
      itemRef,
    };
  },
};
</script>

<template>
  <div class="container">
    <h1>Daftar Item</h1>
    <div class="input-group">
      <input v-model="newItem" placeholder="Tambah item" />
      <button @click="addItem">Tambah</button>
      <button @click="sortItems">Urutkan</button>
    </div>
    <ul>
      <li
        v-for="(item, index) in filteredItems"
        :key="index"
        ref="itemRef"
        class="item"
      >
        {{ item }}
        <button class="delete-btn" @click="removeItem(index)">Hapus</button>
      </li>
    </ul>
    <div class="info">
      <p>Total Item: {{ itemCount }}</p>
      <p>Item Terakhir Ditambahkan: {{ lastAdded || "Belum ada" }}</p>
      <p>Jumlah Karakter Item Terakhir: {{ lastItemLength }}</p>
    </div>
  </div>
</template>

<style>
.container {
  max-width: 400px;
  margin: 0 auto;
  padding: 16px;
  background-color: #f9f9f9;
  border-radius: 8px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
}
h1 {
  margin-bottom: 16px;
  text-align: center;
}
.input-group {
  display: flex;
  gap: 8px;
  margin-bottom: 12px;
}
input {
  flex: 1;
  padding: 8px;
  border-radius: 4px;
  border: 1px solid #ddd;
}
button {
  padding: 8px 12px;
  background-color: #007bff;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}
button:hover {
  background-color: #0056b3;
}
.delete-btn {
  background-color: #dc3545;
}
.delete-btn:hover {
  background-color: #bd2130;
}
.item {
  display: flex;
  justify-content: space-between;
  padding: 8px;
  margin: 4px 0;
  background-color: #e9ecef;
  border-radius: 4px;
}
.info {
  margin-top: 16px;
  font-size: 14px;
  color: #555;
}
</style>