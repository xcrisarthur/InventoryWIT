<template>
    <div>
        <h1>Edit Category Item</h1>
        <div class="container">
            <form @submit.prevent="updateCategory">
                <div class="row row-cols-2 row-cols-lg-2 g-lg-3">
                    <div class="col form-group">
                        <label for="id_kategori">CATEGORY ID</label>
                        <input v-model="categoryItem.id_kategori" type="text" class="form-control" id="id_kategori"
                            disabled>
                    </div>

                    <div class="col form-group">
                        <label for="nama">NAMA</label>
                        <input v-model="categoryItem.nama" type="text" class="form-control" id="nama" required>
                    </div>

                    <div class="col form-group">
                        <button type="submit" class="btn btn-primary">Update Data</button>
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
            categoryItem: {
                id_kategori: '',
                nama: '',
            },
        };
    },
    created() {
        this.fetchcategoryItem();
    },
    methods: {
        fetchcategoryItem() {
            const itemId = this.$route.params.id;
            axios.get(`http://localhost:8080/api/categories/${itemId}`)
                .then(response => {
                    this.categoryItem = response.data.category;
                })
                .catch(error => {
                    console.error('Error fetching categories item:', error);
                });
        },
        updateCategory() {
            axios.put(`http://localhost:8080/api/categories/${this.categoryItem.id_kategori}`, this.categoryItem)
                .then(() => {
                    alert('categories item updated successfully');
                    router.push({ path: `/Kategori` });
                })
                .catch(error => {
                    console.error('Error updating categories item:', error);
                });
        },
    },
};
</script>
    
<style></style>
    