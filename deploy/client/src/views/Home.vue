<script>
import Navbar from "@/components/Navbar.vue"
import Sidebar from "@/components/Sidebar.vue"
import Card from "@/components/Card.vue"
import { mapActions, mapWritableState } from 'pinia';
import { globalStore } from '../stores/global';

export default {
  components: {
    Navbar,
    Sidebar,
    Card
  },

  computed: {
    ...mapWritableState(globalStore, ['books', 'quote', 'isLoading'])
  },

  methods: {
    ...mapActions(globalStore, ['fetchQuote'])
  },

  created(){
    this.fetchQuote()
  }
}
</script>

<template>
  <Navbar />

  <div v-if="isLoading" id="preloader">
    <div class="loading">
        <lottie-player src="https://assets2.lottiefiles.com/packages/lf20_remmdtqv.json" background="transparent" speed="1" style="width: 300px; height: 300px" loop autoplay></lottie-player>
    </div>
  </div>

  <div class="container-fluid">
    <div class="row">
      <Sidebar />
      <main class="col-md-9 ms-sm-auto col-lg-10 px-md-4 py-2">
        <div class="d-flex justify-content-between flex-wrap flex-md-nowrap align-items-center pt-3 pb-2 mb-3 border-bottom">
          <h1 class="h2">Welcome</h1>
        </div>
        <div class="jumbotron">
          <div class="container my-5">
            <div class="row align-items-center g-5">
              <div class="col-lg-6 text-center text-lg-start">
                <h3>"{{ quote.quote }}"</h3>
                <p class="mb-4 pb-2">{{ quote.author }}</p>
              </div>
              <div class="col-lg-6 text-center text-lg-end overflow-hidden">
                <img class="img-fluid" src="https://cdn.pixabay.com/photo/2017/01/31/18/44/bible-2026336__480.png" alt="">
              </div>
            </div>
          </div>
        </div>
        <div class="row g-4">
          <Card v-for="book in books" :key="book.id" :book="book" />
        </div>
      </main>
    </div>
  </div>
</template>

<style>
body {
  font-size: .875rem;
}

.feather {
  width: 16px;
  height: 16px;
  vertical-align: text-bottom;
}

/*
 * Sidebar
 */

.sidebar {
  position: fixed;
  top: 0;
  /* rtl:raw:
  right: 0;
  */
  bottom: 0;
  /* rtl:remove */
  left: 0;
  z-index: 100; /* Behind the navbar */
  padding: 48px 0 0; /* Height of navbar */
  box-shadow: inset -1px 0 0 rgba(0, 0, 0, .1);
}

@media (max-width: 767.98px) {
  .sidebar {
    top: 5rem;
  }
}

.sidebar-sticky {
  position: relative;
  top: 0;
  height: calc(100vh - 48px);
  padding-top: .5rem;
  overflow-x: hidden;
  overflow-y: auto; /* Scrollable contents if viewport is shorter than content. */
}

.sidebar .nav-link {
  font-weight: 500;
  color: #333;
}

.sidebar .nav-link .feather {
  margin-right: 4px;
  color: #727272;
}

.sidebar .nav-link.active {
  color: #2470dc;
}

.sidebar .nav-link:hover .feather,
.sidebar .nav-link.active .feather {
  color: inherit;
}

.sidebar-heading {
  font-size: .75rem;
  text-transform: uppercase;
}

/*
 * Navbar
 */

.navbar-brand {
  padding-top: .75rem;
  padding-bottom: .75rem;
  font-size: 1rem;
  background-color: rgba(0, 0, 0, .25);
  box-shadow: inset -1px 0 0 rgba(0, 0, 0, .25);
}

.navbar .navbar-toggler {
  top: .25rem;
  right: 1rem;
}

.navbar .form-control {
  padding: .75rem 1rem;
  border-width: 0;
  border-radius: 0;
}

.form-control-dark {
  color: #fff;
  background-color: rgba(255, 255, 255, .1);
  border-color: rgba(255, 255, 255, .1);
}

.form-control-dark:focus {
  border-color: transparent;
  box-shadow: 0 0 0 3px rgba(255, 255, 255, .25);
}

#preloader {
  z-index: 1
}

#preloader {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  z-index: 9999;
  background-color: #fff;
  opacity: 0.8;
}

#preloader .loading {
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
}

</style>
