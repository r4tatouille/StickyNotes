<script setup>
import { onMounted, ref } from "vue";
const memos = ref([]);
const showform = ref(false);
const newMemo = ref("");
const toDos = ref();


//ketika tampilan dh keluar
onMounted(() => {
  fetch("https://jsonplaceholder.typicode.com/todos")
    .then((response) => response.json())
    .then((json) => {toDos.value = (json)});
});

/*fungsi tambah memo baru, jadi data apa aja yang ke input 
  yang bakal di tampilkan di aplikasi*/
function addMemo() {
  /*Dibawah maksudnya */
  memos.value.push({
    id: Date.now(),
    /*Dibawah ini maksudnya, data memo nanti di ambil dari 
      objek "newMemo" yang di sematkan objeknya pada text area di bawah */
    memo: newMemo.value,
    /*dibawah ini maksudya info waktu dan tgl di generate
      secara otomatis
      */
    date: new Date().toLocaleDateString("id-ID"),
    backgroundColor: getRandomColor(),
  });
  newMemo.value = "";
  showform.value = false;
}

function getRandomColor() {
  return `#${Math.floor(Math.random() * 16777215).toString(16)}`;
}
</script>
<template>
  <main>
    
    <div class="container">
      <header>
        <h1 class="header-title">Memo</h1>
        <button @click="showform = true" class="header-button">+</button>
      </header>
      <div class="card-container">
        <div v-for="memos in memos" class="card">
          
          <p class="card-content">
            {{ memos.memo }}
          </p>
          <p class="card-date">
            {{ memos.date}}
          </p>
        </div>
      </div>
      <div v-if="showform" class="form-overlay">
        <div class="form-modal">
          <button @click="showform = false" class="form-close-btn">
            &times;
          </button>
          {{ newMemo }}
          <textarea
            v-model="newMemo"
            name="memo"
            id="memo"
            cols="30"
            rows="10"
          ></textarea>
          <button @click="addMemo" class="form-save-btn">Save</button>
        </div>
      </div>
    </div>
  </main>
</template>

<style scoped>
main {
  height: 100vh;
  width: 100vw;
}

.container {
  max-width: 900px;
  padding: 10 px;
  margin: 0 auto;
}

header {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.header-title {
  font-size: 48px;
  font-weight: bold;
  margin-bottom: 25px;
  color: darkblue;
}
.header-button {
  background-color: darkblue;
  border: none;
  padding: 10px;
  width: 50px;
  height: 50px;
  cursor: pointer;
  border-radius: 100%;
  color: white;
}

.card {
  width: 225px;
  height: 225px;
  padding: 10px;
  background-color: bisque;
  margin-bottom: 10px;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  border-radius: 10px;
}

.card-container {
  display: flex;
  flex-wrap: wrap;
  gap: 10px;
}

.form-overlay {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.77);
  z-index: 10;
  display: flex;
  align-items: center;
  justify-content: center;
}

.form-modal {
  width: 420px;
  background-color: white;
  border-radius: 10px;
  padding: 30px;
  position: relative;
  display: flex;
  flex-direction: column;
}

.form-close-btn {
  position: absolute;
  right: 20px;
  top: 5px;
  height: 30px;
  width: 20px;
  background-color: transparent;
  border: none;
  font-size: 25px;
  cursor: pointer;
  border-radius: 20px;
}

.form-save-btn {
  padding: 10px 20px;
  font-size: 20px;
  width: 100%;
  background-color: #495a7d;
  border: none;
  cursor: pointer;
  border-radius: 5px;
  margin-top: 15px;
  color: white;
}
</style>
