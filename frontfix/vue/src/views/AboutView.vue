<template>
  <div id="app">
    <div v-if="daftarData.length > 0">
      <h2>Data Pendaftaran:</h2>
      <ul>
        <li v-for="(data, index) in daftarData" :key="index">
          {{ data.Status }}
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      formData: {
        Nama: "",
        Email: "",
        Asal_Sekolah: "",
      },
      daftarData: [],
    };
  },

  mounted() {
    console.log("Component is mounted.");
    this.fetchData();
  },

  methods: {
    async fetchData() {
      try {
        const response = await fetch("http://localhost:3000/api/daftar");
        if (!response.ok) {
          throw new Error(`HTTP error! Status: ${response.status}`);
        }

        const responseData = await response.json();
        this.daftarData = responseData.docs;
      } catch (error) {
        console.error("Error fetching data:", error);
      }
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

form {
  display: flex;
  flex-direction: column;
  width: 300px;
  margin: auto;
}

input {
  margin-bottom: 15px;
}

button {
  margin-top: 15px;
}
</style>
