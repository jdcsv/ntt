<template>
  <div id="app">
    <h1>Reto técnico - Juan Diego Córdova Brillante</h1>
    <h2>Lista de Usuarios</h2>
    
    <div v-if="loading" class="loading">
      <p>Cargando usuarios...</p>
    </div>

    <div v-else-if="error" class="error">
      <p>{{ error }}</p>
      <button @click="fetchUsers">Reintentar</button>
    </div>

    <!-- Lista de usuarios -->
    <div v-else class="users-container">
      <button @click="fetchUsers" class="reload-btn">Cargar Nuevos Usuarios</button>
      
      <div class="users-grid">
        <div v-for="(user, index) in users" :key="index" class="user-card">
          <img :src="user.picture.large" :alt="user.name.first" class="user-photo">
          <h3>{{ user.name.first }} {{ user.name.last }}</h3>
          <p class="gender">
            <span :class="user.gender">{{ user.gender === 'male' ? 'Masculino' : 'Femenino' }}</span>
          </p>
          <p class="email"> {{ user.email }}</p>
          <p class="info"> {{ user.location.city }}, {{ user.location.country }}</p>
          <p class="info"> {{ formatDate(user.dob.date) }}</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
      users: [],
      loading: false,
      error: null
    }
  },
  mounted() {
    this.fetchUsers()
  },
  methods: {
    async fetchUsers() {
      try {
        this.loading = true
        this.error = null
        
        const response = await fetch('https://randomuser.me/api/?results=10')
        const data = await response.json()
        
        this.users = data.results
      } catch (err) {
        this.error = 'Error al cargar los usuarios'
        console.error(err)
      } finally {
        this.loading = false
      }
    },
    formatDate(dateString) {
      const date = new Date(dateString)
      const day = date.getDate().toString().padStart(2, '0')
      const month = (date.getMonth() + 1).toString().padStart(2, '0')
      const year = date.getFullYear()
      return `${day}/${month}/${year}`
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  padding: 20px;
  max-width: 1200px;
  margin: 0 auto;
}

h1 {
  text-align: center;
  margin-bottom: 30px;
}

.loading, .error {
  text-align: center;
  padding: 20px;
}

.error button, .reload-btn {
  padding: 10px 20px;
  background-color: #42b983;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

.reload-btn {
  display: block;
  margin: 0 auto 20px;
}

.users-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
  gap: 20px;
}

.user-card {
  background: #f9f9f9;
  border: 1px solid #ddd;
  padding: 20px;
  text-align: center;
}

.user-photo {
  width: 80px;
  height: 80px;
  border-radius: 50%;
  margin-bottom: 10px;
}

.user-card h3 {
  margin: 10px 0 5px;
  font-size: 16px;
}

.gender span {
  font-size: 13px;
}

.email {
  color: #666;
  font-size: 13px;
  margin-top: 5px;
}

.info {
  color: #666;
  font-size: 13px;
  margin: 3px 0;
}
</style>