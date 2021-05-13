<template>
  <div class="home">
    <div v-if="projects.length">
      <div v-for="project in projects" :key="project.id">
        <p>{{ project.title }}</p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Home',
  components: {},
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
  }
}
</script>
