<template>
  <div>
    <h1>Create a new Student</h1>
    <form @submit.prevent="create">
      <div>
        username: <input v-model="formData.id" type="text">
      </div>
      <div>
        password: <input v-model="formData.password" type="password">
      </div>
      <div>
        name: <input v-model="formData.name" type="text">
      </div>
      <div>
        email: <input v-model="formData.email" type="email">
      </div>
      <div>
        course code:
        <select v-model="formData.courseCode">
          <option disabled selected>*** Choose a Course ***</option>
          <option v-for="course in courses" :key="course.id" :value="course.id">
            {{ course.name }}
          </option>
        </select>
      </div>
      <p v-show="errorMsg" class="text-danger">
        {{ errorMsg }}
      </p>
      <button type="reset" @click="errorMsg = false">
        RESET
      </button>
      <button @click.prevent="create">
        CREATE
      </button>
    </form>
    <nuxt-link to="/students">
      Back
    </nuxt-link>
  </div>
</template>
<script>
export default {
  data () {
    return {
      formData: {
        id: null,
        password: null,
        name: null,
        email: null,
        courseCode: null
      },
      courses: [],
      errorMsg: false
    }
  },
  created () {
    this.$axios
      .$get('/api/courses')
      .then((courses) => {
        this.courses = courses
      })
  },
  methods: {
    create () {
      this.$axios.$post('/api/students', this.formData)
        .then(() => {
          this.$router.push('/students')
        })
        .catch((error) => {
          this.errorMsg = error.response.data
        })
    }
  }
}
</script>
