<template>
  <div class="app">
      <form @submit.prevent="save">
        <input type="hidden" v-model="form.method">
        <input type="hidden" v-model="form.id">
        <tr>
          <td>NIS</td>
          <td>:</td>
          <td><input type="text" v-model="form.nis"></td>
        </tr>
        <tr>
          <td>Nama</td>
          <td>:</td>
          <td><input type="text" v-model="form.nama"></td>
        </tr>
        <tr>
          <td>Kelas</td>
          <td>:</td>
          <td><input type="text" v-model="form.kelas"></td>
        </tr>
        <tr>
          <td>No Telpon</td>
          <td>:</td>
          <td><input type="text" v-model="form.no_telp"></td>
        </tr>
        <tr>
          <td>Alamat</td>
          <td>:</td>
          <td><input type="text" v-model="form.alamat"></td>
        </tr>
        <tr>
          <td><button type="submit">Submit</button></td>
        </tr>
      </form>
    <table border="1">
      <tr>
        <td>NIS</td>
        <td>Nama</td>
        <td>Kelas</td>
        <td>No Telpon</td>
        <td>Alamat</td>
        <td>Action</td>
      </tr>
      <tr v-for="siswa in siswas" :key="siswa.id">
        <td>{{ siswa.nis }}</td>
        <td>{{ siswa.nama }}</td>
        <td>{{ siswa.kelas }}</td>
        <td>{{ siswa.no_telp }}</td>
        <td>{{ siswa.alamat }}</td>
        <td>
          <button v-on:click="deleteSiswa(siswa.id)">Delete</button>
          <button v-on:click="editSiswa(siswa.id,siswa.nis,siswa.nama,siswa.kelas,siswa.no_telp,siswa.alamat)">Edit</button>
        </td>
      </tr>
    </table>
  </div>
</template>

<script>

import axios from 'axios'
export default {
  name: '#app',
  data() {
    return {
      URL: "http://localhost:8000/api/siswa",
      form: {
        nis: '',
        nama: '',
        kelas: '',
        no_telp: '',
        alamat: '',
        method: 'Save',
        id: '',
      },
      siswas: [],
    }
  },
  mounted(){
    this.load()
  },
  methods: {
    async load(){
      const response = await axios.get(this.URL)
      this.siswas = response.data
    },

    async save() {
      try {
        if(this.form.method == "Save"){
          const response = await axios.post(this.URL,this.form)
          alert("Berhasil Menambahkan Siswa")
        }else if(this.form.method == "Update"){
          const response = await axios.put(this.URL+"/"+this.form.id,this.form)
          alert("Berhasil Mengedit Siswa")
        }
        
        this.form.nis = ''
        this.form.nama = ''
        this.form.kelas = ''
        this.form.no_telp = ''
        this.form.alamat = ''
        this.load();
        
      } catch (error) {
        console.log(error)
      }
    },

    deleteSiswa(id){
      axios.delete(this.URL+"/"+id);
      this.load();
      alert("Berhasil Menghapus Siswa")
    },

    editSiswa(id,nis,nama,kelas,no_telp,alamat){
        this.form.method = "Update"
        this.form.nis = nis
        this.form.nama = nama
        this.form.kelas = kelas
        this.form.no_telp = no_telp
        this.form.alamat = alamat
        this.form.id = id
    }
  },
}
</script>
