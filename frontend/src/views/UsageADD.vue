<template>
  <div>
    <h1>Add Usage Data</h1>

    <div class="container">
      <form @submit.prevent="createUsage">
        <div class="row row-cols-2 row-cols-lg-3 g-lg-3">
          <div class="col">
            <div class="">
              <label for="id_pemakaian" class="form-label">ID PEMAKAIAN</label>
              <input v-model="newUsage.id_pemakaian" type="text" class="form-control" id="id_pemakaian" >
            </div>
          </div>

          <div class="col">
            <div class=""><label for="kode_aset" class="form-label">KODE ASET</label>
              <select v-model="newUsage.kode_aset" class="form-select" id="kode_aset">
                <option v-for="inventorie in inventories" :value="inventorie.kode_aset" :key="inventorie.kode_aset">
                  {{ inventorie.nama }}
                </option>
              </select>
            </div>
          </div>

          <div class="col">
            <div class=""><label for="nomor_induk" class="form-label">NOMOR INDUK</label>
              <select v-model="newUsage.nomor_induk" class="form-select" id="nomor_induk">
                <option v-for="employee in employees" :value="employee.nomor_induk" :key="employee.nomor_induk">
                  {{ employee.nama }}
                </option>
              </select>
            </div>
          </div>

          <div class="col">
            <div class=""><label for="id_ruangan" class="form-label">ID RUANGAN</label>
              <select v-model="newUsage.id_ruangan" class="form-select" id="id_ruangan">
                <option v-for="room in rooms" :value="room.id_ruangan" :key="room.id_ruangan">
                  {{ room.nama }}
                </option>
              </select>
            </div>
          </div>

          <div class="col">
            <div class="">
              <label for="status" class="form-label">STATUS</label>
              <select v-model="newUsage.status" class="form-select" id="status">
                <option value="baik">Baik</option>
                <option value="rusak">Rusak</option>
              </select>
            </div>
          </div>
          

          <div class="col">
            <button type="submit" class="btn btn-primary mt-4">Add Data</button>
          </div>
        </div>
      </form>
    </div>
  </div>
</template>
  
<script>
import axios from 'axios';
import router from '../router';

export default {
  data() {
    return {
      newUsage: {
        id_pemakaian: '',
        kode_aset: '',
        nomor_induk: '',
        id_ruangan: '',
        status: '',
      },
      inventories: [],
      employees: [],
      rooms: [],

    };
  },
  mounted() {
    this.fetchInventories();
    this.fetchEmployees();
    this.fetchRooms();

  },
  methods: {
    createUsage() {
      axios.post('http://localhost:8080/api/usages', this.newUsage)
        // eslint-disable-next-line no-unused-vars
        .then(response => {
          this.newUsage = {};
          router.push({ path: `/Usage` });
        })
        .catch(error => {
          console.error('Error creating usage:', error);
        });
    },
    fetchInventories() {
      axios.get('http://localhost:8080/api/inventories')
        .then(response => {
          this.inventories = response.data.inventory;
        })
        .catch(error => {
          console.error('Error fetching inventories:', error);
        });
    },
    fetchEmployees() {
      axios.get('http://localhost:8080/api/employees')
        .then(response => {
          this.employees = response.data.employees;
        })
        .catch(error => {
          console.error('Error fetching employees:', error);
        });
    },
    fetchRooms() {
      axios.get('http://localhost:8080/api/rooms')
        .then(response => {
          this.rooms = response.data.rooms;
        })
        .catch(error => {
          console.error('Error fetching rooms:', error);
        });
    },
  },
};
</script>
  
<style></style>
  