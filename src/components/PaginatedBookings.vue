<template>
  <div class="">
    
    <table>
      <tr v-for="booking in bookings" :key="booking._id">
        <td>{{booking._id}}</td>
        <td>{{booking.email}}</td>
        <td>
          <router-link :to="`/booking/${booking._id}`">Details</router-link>
        </td>
      </tr>
    </table>
    <br/><br/><br/>
    <button v-for="i in pages" :key="i" @click="fetchPage(i)">{{ i }}</button>
  </div>
</template>

<script>
import { ref, onMounted, computed } from "vue";
export default {
  name: 'PaginatedBookings',
  props: {
    msg: String
  },
  setup() {
    const bookings = ref([]);
    const lastPage = ref(0);
    const perPage = ref(6);

    const pages = computed(() => {
      var pages = [];

      for (var i = 1; i <= lastPage.value; i++) {
        pages.push(i)
      }

      return pages;
    })

    const fetchPage = async function (page) {

      var response = await fetch("/api/bookings?perPage=" + perPage.value + "&page=" + page);

      if (response.ok) {
        var data = await response.json();

        bookings.value = data.bookings;
        lastPage.value = data.pages

      } else {
        alert(response.statusText);
      }
    };

    // onMounted(() => { fetchPage(1) });
    onMounted(function () {
      fetchPage(1);
    });

    return {
      bookings,
      pages,
      fetchPage
    };
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}
</style>
