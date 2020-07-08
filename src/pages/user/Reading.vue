<template>
  <q-page class="flex flex-center">
    <q-card
      class="my-card">
      <q-img
        :src="`${$baseImageURL}/${image}`"
        style="height: 500px"
        basic
      >
        <div class="absolute-bottom text-h6">
          {{ judul }}
        </div>
      </q-img>

      <q-card-section
        v-html="isi">
        {{ isi }}
      </q-card-section>
    </q-card>
  </q-page>
</template>

<script>
export default {
  data () {
    return {
      judul: '',
      isi: '',
      image: ''
    }
  },
  created () {
    this.getData()
  },
  methods: {
    getData () {
      console.log(this.$route.params.id)
      this.$axios.get('berita/tampilsingle/' + this.$route.params.id)
        .then(res => {
          console.log(res)
          this.judul = res.data.data.judul
          this.isi = res.data.data.isi
          this.image = res.data.data.image
        })
    }
  }
}
</script>

<style lang="sass" scoped>
.my-card
  width: 100%
  max-width: 1000px
</style>
