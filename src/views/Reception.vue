<template>
  <div class="reception">
    <table class="table table-striped">
      <thead>
        <tr>
          <th>&nbsp;</th>
          <th>ID</th>
          <th>Name</th>
          <th>Species</th>
          <th>Status</th>
        </tr>
      </thead>
      <tbody>
        <sample v-for="sample in samples" v-bind:key="sample.id" v-bind="sample"></sample>
      </tbody>
    </table>
    <div id="uploadbutton">
      <button v-on:click="upload">Upload</button>
    </div>
  </div>

</template>

<script>
import axios from 'axios'
import Sample from '@/views/Sample'

export default {
  name: 'Reception',
  data () {
    return {
    }
  },
  components: {
    Sample
  },
  methods: {
    upload () {
      let samplesUploaded = []
      this.$children.filter(sample => sample.selected === true).forEach((sample) => {
        this.$store.commit('startSample', sample.id)
        samplesUploaded.push(sample.name)
      })
      alert(`Successful\n\nsamples ${samplesUploaded.join(',')} have been uploaded successfully`)
    }
  },
  computed: {
    samples () {
      return this.$store.getters.samples.filter(sample => sample.status === 'pending')
    }
  },
  created () {
    if (this.$store.getters.samples.length === 0) {
      axios.get('http://localhost:3000/samples/')
        .then(response => {
          this.$store.commit('createSamples', response.data)
        }
        )
    }
  }
}

</script>

<style lang="scss" scoped>
table {
  width: 100%;
}
</style>
