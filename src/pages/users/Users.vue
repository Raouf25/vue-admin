<template>
  <div class="table-responsive">
    <table class="table table-striped table-sm">
      <thead>
      <tr>
        <th>#</th>
        <th>Name</th>
        <th>Email</th>
        <th>Role</th>
        <th>Action</th>
      </tr>
      </thead>
      <tbody>
      <tr v-for="user in users" :key="user.id">
        <td>{{ user.id }}</td>
        <td>{{ user.first_name }} {{ user.last_name }}</td>
        <td>{{ user.email }}</td>
        <td>{{ user.role.name }}</td>
        <td>{{ user.action }}</td>
      </tr>
      </tbody>
    </table>
  </div>

  <ul class="pagination">
    <li class="page-item">
      <a class="page-link" href="javascript:void(0)" @click="prev">Previous</a>
    </li>
    <li class="page-item">
      <a class="page-link" href="javascript:void(0)" @click="next">Next</a>
    </li>
  </ul>

</template>

<script lang="ts">
import {onMounted, ref} from 'vue';
import axios from "axios";

export default {
  name: "Users",
  setup() {

    const users = ref([]);
    const page = ref(1);
    const last_page = ref(0);

    const load = async () => {
      const {data} = await axios.get(`users?page=${page.value}`);
      users.value = data.data;
      last_page.value = data.meta.last_page;
    };

    onMounted(load);

    const next = async () => {
      if (page.value < last_page.value) {
        page.value++;
        await load();
      }
    }

    const prev = async () => {
      if (page.value > 1) {
        page.value--;
        await load();
      }
    }

    return {
      users,
      next,
      prev
    }

  }
}
</script>