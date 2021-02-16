<template>

  <div class="table-responsive">
    <table class="table table-striped table-sm">
      <thead>
      <tr>
        <th>#</th>
        <th>Image</th>
        <th>Title</th>
        <th>Description</th>
        <th>Price</th>
        <th>Action</th>
      </tr>
      </thead>
      <tbody>
      <tr v-for="product in products" :key="product.id">
        <td>{{ product.id }}</td>
        <td><img :src="product.image" width="50"/></td>
        <td>{{ product.description }}</td>
        <td>{{ product.title }}</td>
        <td>{{ product.price }}</td>
        <td>
          <div>
            <router-link :to="`/products/${product.id}/edit`" class="btn btn-sm btn-outline-secondary">Edit</router-link>
            <a href="javascript:void(0)" class="btn btn-sm btn-outline-secondary" @click="del(product.id)">Delete</a>
          </div>
        </td>
      </tr>
      </tbody>
    </table>
  </div>

  <nav>
    <ul class="pagination">
      <li class="page-item">
        <a class="page-link" href="javascript:void(0)" @click="prev">Previous</a>
      </li>
      <li class="page-item">
        <a class="page-link" href="javascript:void(0)" @click="next">Next</a>
      </li>
    </ul>
  </nav>

</template>

<script lang="ts">
import {onMounted, ref, watch} from 'vue'
import axios from 'axios'
import { Product } from '@/models/product';

export default {
  name: "products",
  setup() {
    const products = ref([]);
    const page = ref(1);
    const last_page = ref(0);

   const load = async () => {
      const {data} = await axios.get(`products?page=${page.value}`);
      products.value = data.data;
      last_page.value = data.meta.last_page;
    };

    onMounted(load);

    watch(page, load)

    const next = () => {
      if (page.value < last_page.value) {
        page.value++;
      }
    }

    const prev = () => {
      if (page.value > 1) {
        page.value--;
      }
    }

    const del = async (id: number) => {
      if (confirm('Are you sure?')) {
        await axios.delete(`products/${id}`);
        products.value = products.value.filter((r: Product) => r.id !== id);
      }
    };

    return {
      products,
      del,
      next,
      prev
    }
  }
}
</script>

<style scoped>

</style>
