<template>
  <home-view>
    <div class="upload-files">
      <h2>Загрузите файл</h2>
      <div class="upload">
        <input type="file" ref="fileInput" multiple @change="handleFileChange">
        <button @click="uploadFiles">Загрузить файл</button>
        <div v-if="errorMessage">{{ errorMessage }}</div>
      </div>
    </div>
  </home-view>
</template>

<script>
import axios from 'axios';
import HomeView from "@/views/HomeView.vue";

export default {
  components: {HomeView},
  data() {
    return {
      files: [],
      errorMessage: '',
    };
  },
  methods: {
    handleFileChange(event) {
      this.files = event.target.files;
      // Сброс флага при изменении списка файлов
    },
    async uploadFiles() {
      const formData = new FormData();
      for (let i = 0; i < this.files.length; i++) {
        formData.append('files[]', this.files[i]);
      }

      try {
        const response = await axios.post('http://file-hosting.ru/api-file/files', formData, {
          headers: {
            'Content-Type': 'multipart/form-data',
            'Authorization': `Bearer ${localStorage.getItem('token')}`,
          }
        });

        // Обработка успешной загрузки файлов
        console.log('Files uploaded successfully:', response.data);
        this.$router.push('/user-files');
      } catch (error) {
        // Обработка ошибок при загрузке файлов
        this.errorMessage = 'Error uploading files. Please try again.';
        console.error('Error uploading files:', error);
      }
    }
  }
};
</script>

<style scoped>
.upload-files {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  background-color: #FFD14F77;
  color: #fff;
  height: 70vh;
}

h2 {
  color: White;
  text-align: center;
  text-transform: uppercase;
}

.upload {
  display: flex;
  justify-content: space-between;
  align-items: center;
  background-color: #FFE0BBE4;
  padding: 20px;
  border-radius: 10px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.3);
  min-width: 500px;
  width: max-content;
  margin-top: 20px;
}
.success-message{
  text-align: center;
  margin-top: 5px;
  padding: 5px;
}
h2 {
  color: #fff;
  text-align: center;
  margin-bottom: 20px;
}

label {
  color: #fff;
}

button {
  width: calc(50% - 5px);
  padding: 10px;
  border: none;
  border-radius: 5px;
  background-color: green;
  color: #fff;
  cursor: pointer;
  transition: background-color 0.3s;
}

button:hover {
  background-color: lightgreen;
}

</style>