<script>
import { mapActions, mapWritableState } from 'pinia';
import { globalStore } from '../stores/global';
import Swal from 'sweetalert2';

export default {
   computed: {
    ...mapWritableState(globalStore, ['isLogin', 'nickname', 'query', 'totalItems', 'books'])
   },

   methods: {
    ...mapActions(globalStore, ['fetchBooks', 'fetchQuote']),

    logout(){
        Swal.fire({
        title: 'Are you sure want to quit?',
        icon: 'warning',
        showCancelButton: true,
        confirmButtonColor: '#3085d6',
        cancelButtonColor: '#d33',
        confirmButtonText: 'Yes, quit now!'
      }).then((result) => {
        if (result.isConfirmed) {
          localStorage.clear()
          google.accounts.id.disableAutoSelect()
          this.isLogin = false
          this.nickname = ""
          this.query = ""
          this.books = []
          this.$router.replace("/login")
        }
      })
    },

    clickSearch(){
        this.fetchBooks()
        this.query = ""
    },

    clickMovePage(){
        this.fetchQuote()
        this.$router.push("/")
    }
   },

   created(){
    if(localStorage.access_token){
        this.nickname = localStorage.name
        this.isLogin = true
    } else {
        this.isLogin = false
    }
   }
}
</script>

<template>
    <header class="navbar navbar-dark sticky-top bg-dark flex-md-nowrap p-0 shadow">
        <a @click.prevent="clickMovePage" class="navbar-brand col-md-3 col-lg-2 me-0 px-3" href="#">
            <img src="https://cdn.pixabay.com/photo/2014/03/25/16/34/reading-297450__480.png" alt="ReadingShed" width="50">
            <span class="ms-2"> Reading Shed </span>
        </a>
        <button class="navbar-toggler position-absolute d-md-none collapsed" type="button" data-bs-toggle="collapse" data-bs-target="#sidebarMenu" aria-controls="sidebarMenu" aria-expanded="false" aria-label="Toggle navigation">
            <span class="navbar-toggler-icon"></span>
        </button>
        <form v-if="$route.fullPath == '/'" @submit.prevent="clickSearch" class="form-control form-control-dark w-100 d-flex flex-row">
            <input v-model="query" class="form-control form-control-dark" type="text" placeholder="Search" aria-label="Search" required>
            <button class="btn btn-outline-light rounded ms-1" type="submit"><i class="fa-solid fa-magnifying-glass"></i></button>
        </form>
        <div v-if="isLogin" class="navbar-nav">
            <div class="nav-item text-nowrap">
                <a @click.prevent="logout" class="nav-link px-3" href="#">Sign out</a>
            </div>
        </div>
    </header>
</template>