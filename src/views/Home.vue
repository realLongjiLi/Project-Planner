<template>
  <div class="home">
    <div v-if="projects.length">
      <div v-for="project in projects" :key="project.id">
        <SingleProject :project="project" @delete="deleteHandler" />
      </div>
    </div>
  </div>
</template>

<script>
import SingleProject from '../components/SingleProject'
export default {
  name: 'Home',
  components: { SingleProject },
  data() {
    return {
      projects: []
    }
  },
  async mounted() {
    const res = await fetch('http://localhost:3000/projects')
    if (!res.ok) {
      const msg = `An error has occured: ${res.status}`
      throw new Error(msg)
    }
    const projects = await res.json()
    this.projects = projects
  },
  methods: {
    deleteHandler(id) {
      this.projects = this.projects.filter((project) => project.id != id)
    }
  }
}
</script>
