<template>
<div class="card shadow mt-3">
  <div class="card-body">
    <h5 class="card-title">Edit History</h5>
     <form class="row g-3" @submit.prevent="update">
  <div class="col-md-6">
    <label for="inputEmail4" class="form-label">SD</label>
    <input type="text" class="form-control" id="inputEmail4" 
    v-model="historys.sd" />
      <div class="alert alert-danger" v-if="validation.sd">
        {{ validation.sd[0] }}
      </div>
  </div>
  <div class="col-md-6">
    <label for="inputEmail4" class="form-label">SMP</label>
    <input type="text" class="form-control" id="inputEmail4" 
    v-model="historys.smp" />
      <div class="alert alert-danger" v-if="validation.smp">
        {{ validation.smp[0] }}
      </div>
  </div>
  <div class="col-md-6">
    <label for="inputPassword4" class="form-label">SMA</label>
    <input type="text" class="form-control" id="inputPassword4"
    v-model="historys.sma"/>
    <div class="alert alert-danger" v-if="validation.sma">
        {{ validation.sma[0] }}
      </div>
  </div>
  <div class="col-md-6">
    <label for="inputAddress" class="form-label">Perguruan Tinggi</label>
    <input type="text" class="form-control" id="inputAddress" 
    v-model="historys.perguruan_tinggi" />
    <div class="alert alert-danger" v-if="validation.perguruan_tinggi">
        {{ validation.perguruan_tinggi[0] }}
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
    const historys = reactive({
      sd: '',
      smp: '',
      sma: '',
      perguruan_tinggi: ''
    })
    const validation = ref([]);
    const router = useRouter();
    const route = useRoute()
    onMounted(()=>{
      axios.get(`http://127.0.0.1:8001/api/historys/${route.params.id}`)
      .then(response => {
        console.log(response.data.data.sd)
        historys.sd = response.data.data.sd
        historys.smp = response.data.data.smp
        historys.sma = response.data.data.sma
        historys.perguruan_tinggi = response.data.data.perguruan_tinggi
      }).catch(error =>{
        console.log(error.response.data)
      })
    })
    function update(){
      let sd = historys.sd
      let smp = historys.smp
      let sma = historys.sma
      let perguruan_tinggi = historys.perguruan_tinggi
      axios.put(`http://127.0.0.1:8001/api/historys/${route.params.id}`, {
        sd: sd,
        smp: smp,
        sma: sma,
        perguruan_tinggi: perguruan_tinggi
      })
      .then(() => {
        router.push({
          name:'historys'
        })
      }).catch(error => {
        console.log(error)
      })
    }
    return {
      historys,
      validation,
      router, 
      update,
      route
    }
  },
}
</script>