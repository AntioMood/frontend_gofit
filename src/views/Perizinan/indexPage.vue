<template>
    <div class="d-flex justify-content-between flex-wrap flex-md-nowrap align-items-center pt-3 pb-2 mb-3 border-bottom">
        <h1 class="h2">Dashboard</h1>
    </div>

    <div class="container mt-5">
        <div class="row">
            <div class="col-md-12">
                <div class="card border-0 rounded shadow">
                    <div class="card-body">
                        <router-link :to="{name: 'perizinanAll.index'}" class="btn btn-md btn-success">Data Perizinan All</router-link>
                        <table class="table table-striped table-bordered mt-4">

                            <thead class="thead-dark">
                                <tr>
                                    <th scope="col">Nama Instruktur</th>
                                    <th scope="col">Nama Kelas</th>
                                    <th scope="col">Hari</th>
                                    <th scope="col">Tanggal Pengajuan Izin</th>
                                    <th scope="col">Tanggal Mengisi kelas</th>
                                    <th scope="col">Jam</th>
                                    <th scope="col">Nama Instruktur Pengganti</th>
                                    <th scope="col">Status</th>
                                </tr>
                            </thead>
                            <tbody>
                                <tr v-for="(perizinan, id_perizinan) in perizinans" :key="id_perizinan">
                                    <td>{{ perizinan.nama }}</td>
                                    <td>{{ perizinan.nama_kelas }}</td>
                                    <td>{{ perizinan.hari }}</td>
                                    <td>{{ perizinan.tgl_izin }}</td>
                                    <td>{{ perizinan.tanggal }}</td>
                                    <td>{{ perizinan.jam_mulai }} - {{ perizinan.jam_selesai }}</td>
                                    <td>{{ perizinan.nama_pengganti }}</td>
                                    <td>{{ perizinan.status }}</td>
                                    <td class="text-center">
                                        <!-- <router-link :to="{ name: 'jadwalU.edit', params: { id_jadwalU: jadwal_umum.id_jadwalU } }" class="btn btn-sm btn-primary mr-1">
                                            EDIT
                                        </router-link> -->
                                        <button @click.prevent="konfirmasi(perizinan.id_perizinan)" class="btn btn-sm btn-primary mr-1">
                                            Konfirmasi
                                        </button>
                                    </td>
                                </tr>
                            </tbody>
                        </table>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios'
import { onMounted, ref } from 'vue'
import Swal from "sweetalert2";

export default {
    setup() {
        //reactive state
        let perizinans = ref([])

        //mounted
        onMounted(() => {
            //get API from Laravel Backend
            axios.get('https://pandu.ppcdeveloper.com/api/show_izin')
            .then(response => {
                //assign state posts with response data
                perizinans.value = response.data.data
            }).catch(error => {
            console.log(error.response.data)
            })
        })

        function konfirmasi(id_perizinan){
          axios.post(`https://pandu.ppcdeveloper.com/api/perizinan/${id_perizinan}`)
            .then(() => {     
            //splice instruktur 
                // window.location.reload();
                Swal.fire('Berhasil!')
            }).catch(error =>{
                console.log(error.response.data)
            })
        }

        return{
            perizinans,
            konfirmasi
        }
    }
}
</script>

<style>

</style>