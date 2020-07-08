<template>
  <q-page padding>
    <q-card flat >
      <q-card-section class="q-gutter-md">
        <div class="text-h5">Edit Berita</div>
        <q-form
          @submit="onSubmit"
          @reset="onReset"
          class="q-gutter-md"
          >
          <div class="q-pa-md">
            <q-input
            filled
            v-model="form.tag"
            label="Tag"
            lazy-rules
            :rules="[ val => val && val.length > 0 || 'Please type something']"
            />

            <q-input
              filled
              v-model="form.judul"
              label="Judul"
              lazy-rules
              :rules="[ val => val && val.length > 0 || 'Please type something']"
            />

            <q-editor
              filled
              autogrow
              v-model="form.isi"
              min-height="5rem" />

            <q-file class="q-pt-md" accept=".jpg, image/*" color="teal" filled v-model="image" label="Upload Gambar">
                <template v-slot:prepend>
                  <q-icon name="cloud_upload"/>
                </template>
            </q-file>
          </div>
          <div>
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
      form: {
        tag: null,
        judul: null,
        isi: ''
      },
      image: null
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
          this.form = res.data.data
        })
    },
    onReset () {
      this.tag = null
      this.judul = null
      this.isi = null
    },
    onSubmit () {
      const formData = new FormData()
      formData.append('image', this.image)
      formData.append('data', JSON.stringify(this.form))
      this.$axios.put(`berita/edit/${this.$route.params.id}`, formData)
        .then(res => {
          if (res.data.sukses) {
            this.$q.notify({
              type: 'positive',
              message: res.data.pesan,
              position: 'top'
            })
            this.$router.push({ name: 'data' })
          } else {
            this.$q.notify({
              type: 'negative',
              message: res.data.pesan,
              position: 'top'
            })
          }
        })
    }
  }
}
</script>
