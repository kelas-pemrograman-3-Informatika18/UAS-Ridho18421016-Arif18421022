<template>
  <q-page>
    <div class="q-pa-md">
      <!--Animasi Atas-->
      <q-carousel
        animated
        vertical
        v-model="slide"
        navigation
        infinite
        autoplay
        transition-prev="slide-right"
        transition-next="slide-left"
        height="400px">
        <q-carousel-slide name="first" img-src="https://media.suara.com/pictures/970x544/2016/10/11/o_1aupdku9vbu7k7t97q120p8la.jpg">
          <div class="absolute-bottom custom-caption">
            <div class="text-h5">Seorang Dosen membuat mahasiswanya kewalahan</div>
            <div class="text-header">Kejadian bermula saat Dosen berinisial 'W' , membuat Uts untuk mahasiswanya tetapi , tugasnya sungguh berat , dan materi yang dipelajari..</div>
          </div>
        </q-carousel-slide>
        <q-carousel-slide name="second" img-src="http://fik.ubl.ac.id/wp-content/uploads/2019/10/WhatsApp-Image-2019-10-21-at-09.05.51.jpeg">
          <div class="absolute-bottom custom-caption">
            <div class="text-h5">Mahasiswa Fakultas Ilmu Komputer Raih Juara 2 Lomba Kontes Robot Nusantara (KRON)</div>
            <div class="text-header">Mahasiswa Fakultas Ilmu Komputer Program Studi Informatika meraih juara ke 2 pada ajang lomba KRON “6th Kontes Robot Nusantara” yang diadakan oleh IYRA (International Youth Robot Association) dan dilaksanakan di UIN Syarif Hidayatullah Jakarta.</div>
          </div>
        </q-carousel-slide>
      </q-carousel>
    </div>
    <!--Isi Home-->
    <q-separator style="height: 2px" class="bg-blue"/>
    <br>
    <!--Daftar Berita-->
    <div class="row">
      <div class="col">
        <q-card
          class="my-card"
          flat
          bordered
          v-for="now in now"
          :key="now.judul"
          :to="now.to"
          exact>
          <q-card-section horizontal>
            <q-card-section class="col q-pt-xs">
              <div class="text-overline">{{ now.tag }}</div>
              <div class="text-h5 q-mt-sm q-mb-xs">{{ now.judul }}</div>
            </q-card-section>

            <q-card-section class="col-5 flex flex-center">
              <q-space/>
              <q-img
                class="rounded-borders"
                :src="now.gambar"
                style="height: 150px; width:150px"
              />
            </q-card-section>
          </q-card-section>
          <q-separator />
            <q-card-actions>
              <q-btn flat color="primary" :to="{ name: 'readuser', params: { id: now._id } }" >
                See More
              </q-btn>
            </q-card-actions>
          <q-separator />
        </q-card>
      </div>
    </div>
  </q-page>
</template>

<script>
export default {
  data () {
    return {
      tag: null,
      judul: null,
      isi: null,
      slide: 1,
      now: []
    }
  },
  created () {
    this.getData()
  },
  methods: {
    getData () {
      this.$axios.get('berita/tampil')
        .then(res => {
          console.log(res)
          if (res.data.sukses) {
            this.now = res.data.data
          } else {
            this.$q.notify({
              type: 'negative',
              message: res.data.pesan
            })
          }
        })
    },
    confirm (id) {
      this.$q.dialog({
        title: 'Konfirmasi',
        message: 'Apakah Anda Yakin Menghapus Data',
        cancel: true,
        persistent: true
      }).onOk(() => {
        console.log('>>>> OK' + id)
        this.$axios.delete('kegiatan/delete/' + id)
          .then(res => {
            if (res.data.sukses) {
              this.$q.notify({
                type: 'positive',
                message: res.data.pesan
              })
              this.getData()
            } else {
              this.$q.notify({
                type: 'negative',
                message: res.data.pesan
              })
            }
          })
      })
    }
  }
}
</script>
<style lang="sass" scoped>
.custom-caption
  text-align: center
  padding: 12px
  color: white
  background-color: rgba(0, 0, 0, .3)
</style>
