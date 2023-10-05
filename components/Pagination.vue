<template>
  <v-row>
      <div class="pagination" id="container">
          <a href="#" @click="getFirstPage()">&laquo;&laquo;</a>
          <a href="#" @click="getPrevPage()">&laquo;</a>
          <a v-for="index in totalItems" :key="index"
              :class="index === getCurrentPage()
              ? 'active' : 'notActive'" href="#" 
              @click="setPage(index)">{{ index }}</a>
          <a href="#" @click="getNextPage()">&raquo;</a>
          <a href="#" @click="getLastPage()">&raquo;&raquo;</a>
      </div>
  </v-row>
</template>

<script>
import { ref } from 'vue';

var lastTwo = ref(3);
var firstTwo = ref(0);
var totalItems = ref(10);
var currentPage = ref(1);
export default {
  props: {
      currentPage: Number, 
      totalItems: Number //is from the server
  },
  data() {
      return {
          totalItems,
          firstTwo,
          lastTwo: this.totalItems+1,
          currentPage
      }
  },
  methods: {
      getNextPage() {
          if (this.currentPage == this.getTotalPages()) {
              this.writeDebugCode('getNextPage');
              this.currentPage = this.getTotalPages();
          } else {
              this.currentPage ++
          }
      },
      getPrevPage() {
          if (this.getCurrentPage() <= 1) {
              this.currentPage = 1;
          }
          else this.currentPage--
      },
      getFirstPage() {
          this.currentPage  = 1;
      },
      getTotalPages() {
          return totalItems.value;
      },
      getLastPage() {
        console.log(this.currentPage)
          this.currentPage = this.getTotalPages();
          console.log(this.currentPage)
      },
      range(min, max) {
          var array = [], j = 0;
          min = this.currentPage - 1, max = this.currentPage + 1

          if (max >= this.getTotalPages()) {
              max = this.getTotalPages() - 1;
          }
          console.log('Legkisebb: ' + min)
          console.log('max: ' + max)
          if (this.currentPage >= 2 && 
              this.currentPage <= 
              this.getTotalPages()) {
              min = min - 1
          }
          for (var i = min; i <= max; i++) {
              array[j] = i;
              j++;
          }
          return array;
      },
      setPage(page) {
          this.currentPage = page
      },
      getCurrentPage() {
          return this.currentPage;
      }
    }
}
</script>
<style>

#container{
  padding-top:20px;
  margin-top:20px;
  width:100vw;
  color:white;
  display:flex;
  align-items:center;
}
.pagination {
    display: inline-block;
    margin-bottom: 150px;
}

footer {
    display: flex;
    background-color: #42b942;
    height: 50px;
    width: 100%;
    position: fixed;
}

.pagination a {
    color: black;
    float: left;
    margin-top: 40px;
    margin-inline: auto;
    padding: 8px 16px;
    text-decoration: none;
    align-items: center;
    transition: background-color .3s;
}

.pagination a.notActive {
    color: black;
}

/* Style the active/current navigation */
.pagination a.active {
    background-color: dodgerblue;
    color: white;
}

/* Add a grey background color on mouse-over */
.pagination a:hover:not(.active) {
    background-color: #ddd;
}
</style>
