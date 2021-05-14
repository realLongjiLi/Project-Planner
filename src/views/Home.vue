<template>
  <div class="home">
    <FilterNav @filterChange="curr_filter = $event" />
    <div v-if="projects.length">
      <div v-for="project in filteredProjects" :key="project.id">
        <SingleProject
          :project="project"
          @delete="deleteHandler"
          @complete="completeHandler"
        />
      </div>
    </div>
  </div>
</template>

<script>
import SingleProject from '../components/SingleProject'
import FilterNav from '../components/FilterNav'
export default {
  name: 'Home',
  components: { SingleProject, FilterNav },
  data() {
    return {
      projects: [],
      curr_filter: 'all'
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
    },
    completeHandler(id) {
      let project = this.projects.find((p) => p.id === id)
      project.complete = !project.complete
    }
  },
  computed: {
    filteredProjects() {
      if (this.curr_filter === 'completed') {
        return this.projects.filter((p) => p.complete)
      } else if (this.curr_filter === 'ongoing') {
        return this.projects.filter((p) => !p.complete)
      } else {
        return this.projects
      }
    }
  }
}
</script>
