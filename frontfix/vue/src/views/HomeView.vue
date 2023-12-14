<template>
  <div id="app">
    <router-link to="/about">About</router-link>
    <form @submit.prevent="submitForm">
      <input v-model="formData.Nama" type="text" placeholder="Nama" required />
      <input
        v-model="formData.Email"
        type="email"
        placeholder="Email"
        required
      />
      <input
        v-model="formData.Asal_Sekolah"
        type="text"
        placeholder="Asal Sekolah"
        required
      />

      <button type="submit">Submit</button>
    </form>

    <div v-if="daftarData.length > 0">
      <h2>Data Pendaftaran:</h2>
      <ul>
        <li v-for="(data, index) in daftarData" :key="index">
          {{ data.Nama }}
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

    async submitForm() {
      try {
        const response = await fetch("http://localhost:3000/api/daftar", {
          method: "POST",
          headers: {
            "Content-Type": "application/json",
          },
          body: JSON.stringify({
            docs: [this.formData],
          }),
        });

        if (!response.ok) {
          throw new Error(`HTTP error! Status: ${response.status}`);
        }

        const responseData = await response.json();
        console.log("Response:", responseData);

        // Menambahkan data baru ke daftar yang ada
        this.daftarData.push(responseData.docs[0]);

        this.formData = {
          Nama: "",
          Email: "",
          Asal_Sekolah: "",
        };
      } catch (error) {
        console.error("Error submitting form:", error);
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
