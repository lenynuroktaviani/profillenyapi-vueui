<template>
<div class="card shadow mt-3">
  <div class="card-body">
    <h5 class="card-title">Edit Profil</h5>
     <form class="row g-3" @submit.prevent="update">
  <div class="col-md-6">
    <label for="inputEmail4" class="form-label">NAMA</label>
    <input type="text" class="form-control" id="inputEmail4" 
    v-model="kontaks.nama" />
      <div class="alert alert-danger" v-if="validation.nama">
        {{ validation.nama[0] }}
      </div>
  </div>
  <div class="col-md-6">
    <label for="inputEmail4" class="form-label">TEMPAT TANGGAL LAHIR</label>
    <input type="text" class="form-control" id="inputEmail4" 
    v-model="kontaks.ttl" />
      <div class="alert alert-danger" v-if="validation.ttl">
        {{ validation.ttl[0] }}
      </div>
  </div>
  <div class="col-md-6">
    <label for="inputPassword4" class="form-label">alamat</label>
    <input type="text" class="form-control" id="inputPassword4"
    v-model="kontaks.alamat"/>
    <div class="alert alert-danger" v-if="validation.alamat">
        {{ validation.alamat[0] }}
      </div>
  </div>
  <div class="col-md-6">
    <label for="inputAddress" class="form-label">Jenis Kelamin</label>
    <input type="text" class="form-control" id="inputAddress" 
    v-model="kontaks.jk" />
    <div class="alert alert-danger" v-if="validation.jk">
        {{ validation.jk[0] }}
      </div>
  </div>

  <div class="col-12">
    <button type="submit" class="btn btn-primary">Edit</button>
  </div>
</form>
  </div>
</div>

</template>
<script>
import { onMounted, ref } from 'vue';
import { reactive } from 'vue';
import { useRouter, useRoute } from 'vue-router'
import axios from 'axios'
export default {
  setup() {
    const kontaks = reactive({
      nama: '',
      ttl: '',
      alamat: '',
      jk: ''
    })
    const validation = ref([]);
    const router = useRouter();
    const route = useRoute()
    onMounted(()=>{
      axios.get(`http://127.0.0.1:8001/api/kontaks/${route.params.id}`)
      .then(response => {
        console.log(response.data.data.sd)
        kontaks.nama = response.data.data.nama
        kontaks.ttl = response.data.data.ttl
        kontaks.alamat = response.data.data.alamat
        kontaks.jk = response.data.data.jk
      }).catch(error =>{
        console.log(error.response.data)
      })
    })
    function update(){
      let nama = kontaks.nama
      let ttl = kontaks.ttl
      let alamat = kontaks.alamat
      let jk = kontaks.jk
      axios.put(`http://127.0.0.1:8001/api/kontaks/${route.params.id}`, {
        nama: nama,
        ttl: ttl,
        alamat: alamat,
        jk: jk
      })
      .then(() => {
        router.push({
          name:'Kontaks'
        })
      }).catch(error => {
        console.log(error)
      })
    }
    return {
      kontaks,
      validation,
      router, 
      update,
      route
    }
  },
}
</script>