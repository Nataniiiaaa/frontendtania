<template>
  <div class="container mt-5">
    <div class="card shadow p-4 mb-3 bg-white rounded">
      <div class="card-body">
        <h2 class="card-title">Detail Data Matakuliah</h2>
        <ul class="list-group">
          <li class="list-group-item">
            <strong>Kode Matakuliah:</strong> {{ data.kode }}
          </li>
          <li class="list-group-item">
            <strong>Nama Matakuliah:</strong> {{ data.namamatakuliah }}
          </li>
          <li class="list-group-item">
            <strong>SKS:</strong> {{ data.sks }}
          </li>
          <li class="list-group-item">
            <strong>Semester:</strong> {{ data.semester }}
          </li>
          <!-- Add more details if needed -->
        </ul>
        <div class="btn-group mt-3">
          <router-link class="btn btn-danger" to="/matakuliah"
            >Kembali</router-link
          >
        </div>
      </div>

      <div class="table-responsive shadow p-3 mb-5 bg-white rounded">
        <table class="table table-bordered table-striped">
          <thead class="thead-dark">
            <tr>
              <th scope="col">Tahun</th>
              <th scope="col">Semester</th>
              <th scope="col">NIM</th>
              <th scope="col">Nama Mahasiswa</th>
              <th scope="col">Nilai</th>
              <th scope="col">Predikat</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="data in DataDetilKrs" :key="data.id">
              <template v-if="data.matakuliah_id == this.matakuliahId">
                <td>{{ getTahunKrs(data.krs_id) }}</td>
                <td>{{ getSemesterKrs(data.krs_id) }}</td>
                <td>{{ getNimMahasiswa(data.mahasiswa_id) }}</td>
                <td>{{ getNamaMahasiswa(data.mahasiswa_id) }}</td>
                <td>{{ data.nilai }}</td>
                <td>{{ getPredikat(data.nilai) }}</td>
              </template>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "redaxios";

export default {
  name: "DetailMatakuliah",
  data() {
    return {
      data: null,
      matakuliahId: this.$route.params.id,
      Matakuliah: {
        id: "",
        kode: '',
        namamatakuliah: "",
        sks: "",
        semester: "",
      
      },
      DataDetilKrs: [],
      allDetilKrs: [],
      KRSList: [],
      MahasiswaList: [],
      MatakuliahList: [],
    };
  },
  created() {
    this.fetchMatakuliahData();
    this.loadAllDetilKrs()
    this.loadKrsList()
    this.loadMahasiswaList()
    this.loadMatakuliahList()
  },
  methods: {
    fetchMatakuliahData() {
      var url = `https://api-group7-prognet.manpits.xyz/api/matakuliah/${this.matakuliahId}`;
      axios.get(url).then(({ data }) => {
        console.log(data);
        this.data = data;
      });
    },    
    loadAllDetilKrs() {
      var url = 'https://api-group7-prognet.manpits.xyz/api/detilkrs';
      axios.get(url).then(({ data }) => {
        this.DataDetilKrs = data;
      });
    },
    loadKrsList() {
      var krsurl = 'https://api-group7-prognet.manpits.xyz/api/krs';
      axios.get(krsurl).then(({ data }) => {
        console.log(data);
        this.KRSList = data;
      });
    },
    loadMatakuliahList() {
      var matkulurl = 'https://api-group7-prognet.manpits.xyz/api/matakuliah';
      axios.get(matkulurl).then(({ data }) => {
        console.log(data);
        this.MatakuliahList = data;
      });
    },
    loadMahasiswaList() {
      var mhsurl = 'https://api-group7-prognet.manpits.xyz/api/mahasiswa';
      axios.get(mhsurl).then(({ data }) => {
        console.log(data);
        this.MahasiswaList = data;
      });
    },
    getTahunKrs(krs_id) {
      const krs = this.KRSList.find((krs) => krs.id === krs_id);
      return krs ? krs.tahun : 'Tidak Diketahui';
    },
    getSemesterKrs(krs_id) {
      const krs = this.KRSList.find((krs) => krs.id === krs_id);
      return krs ? krs.semester : 'Tidak Diketahui';
    },
    getKodeMatkul(matkuliah_id) {
      const matakuliah = this.MatakuliahList.find((matakuliah) => matakuliah.id === matkuliah_id);
      return matakuliah ? matakuliah.kode : 'Tidak Diketahui';
    },
    getNamaMatkul(matkuliah_id) {
      const matakuliah = this.MatakuliahList.find((matakuliah) => matakuliah.id === matkuliah_id);
      return matakuliah ? matakuliah.namamatakuliah : 'Tidak Diketahui';
    },
    getNimMahasiswa(mahasiswa_id) {
      const mahasiswa = this.MahasiswaList.find((mahasiswa) => mahasiswa.id === mahasiswa_id);
      return mahasiswa ? mahasiswa.nim : 'Tidak Diketahui';
    },
    getNamaMahasiswa(mahasiswa_id) {
      const mahasiswa = this.MahasiswaList.find((mahasiswa) => mahasiswa.id === mahasiswa_id);
      return mahasiswa ? mahasiswa.nama : 'Tidak Diketahui';
    },

    getPredikat(nilai) {
      if (nilai > 0 && nilai < 45) return "E";
      else if (nilai >= 45 && nilai < 50) return "D";
      else if (nilai >= 50 && nilai < 55) return "D+";
      else if (nilai >= 55 && nilai < 60) return "C";
      else if (nilai >= 60 && nilai < 65) return "C+";
      else if (nilai >= 65 && nilai < 75) return "B";
      else if (nilai >= 75 && nilai < 80) return "B+";
      else if (nilai >= 80 && nilai <= 100) return "A";
    },
  },
};
</script>
