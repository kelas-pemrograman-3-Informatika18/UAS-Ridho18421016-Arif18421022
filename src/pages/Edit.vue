<template>
  <q-page padding>
    <q-card flat >
      <q-card-section class="q-gutter-md">
        <div class="text-h5">Edit Berita</div>
        <q-form
          @reset="onReset"
          @submit="onSubmit"
          class="q-mt-lg"
        >
        <div class="q-gutter-md">
          <q-input label="Tag" v-model="tag" filled  :rules="[ val => val && val.length > 0 || 'Insert Your Time']"></q-input>
          <q-input label="Judul Berita" v-model="judul" filled :rules="[ val => val && val.length > 0 || 'Insert Your Judul']"></q-input>
          <q-input label="Isi" v-model="isi" filled :rules="[ val => val && val.length > 0 || 'Insert Your Keterangan']"></q-input>
          <q-btn label="Update" type="submit" color="light-blue-12" unelevated></q-btn>
          <q-btn label="Reset" type="reset" color="light-blue-12" flat unelevated></q-btn>
        </div>
        </q-form>
      </q-card-section>
    </q-card>
  </q-page>
</template>
<script>
export default {
  data () {
    return {
      tag: null,
      judul: null,
      isi: null
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
          const data = res.data
          this.tag = data.tag
          this.judul = data.judul
          this.isi = data.isi
        })
    },
    onReset () {
      this.tag = null
      this.judul = null
      this.isi = null
    },
    onSubmit () {
      this.$axios.put('berita/edit/' + this.$route.params.id, {
        waktu: this.waktu,
        judul: this.judul,
        keterangan: this.keterangan
      }).then(res => {
        if (res.data.sukses) {
          this.$q.notify({
            type: 'positive',
            message: res.data.pesan
          })
          this.$router.push({ name: 'data' })
        } else {
          this.$q.notify({
            type: 'negative',
            message: res.data.pesan
          })
        }
      })
    }
  }
}
</script>
