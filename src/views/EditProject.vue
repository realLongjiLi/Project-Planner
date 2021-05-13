<template>
  <form @submit.prevent="submitHandler">
    <label>Title</label>
    <input type="text" v-model="title" required />
    <label>Details</label>
    <textarea v-model="details"></textarea>
    <button>Update Project</button>
  </form>
</template>

<script>
export default {
  props: ['id'],
  data() {
    return {
      title: '',
      details: '',
      uri: 'http://localhost:3000/projects/' + this.id
    }
  },
  async mounted() {
    const res = await fetch(this.uri)
    if (!res.ok) {
      const msg = `An error has occured: ${res.status}`
      throw new Error(msg)
    }
    const p = await res.json()
    this.title = p.title
    this.details = p.details
  },
  methods: {
    async submitHandler() {
      const res = await fetch(this.uri, {
        method: 'PATCH',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({
          title: this.title,
          details: this.details
        })
      })
      if (!res.ok) {
        const msg = `An error has occured: ${res.status}`
        throw new Error(msg)
      }
      this.$router.push('/')
    }
  }
}
</script>

<style></style>
