<template>
  <div id="app">  
    <p>
      <button class="button" @click="prevPage">Previous</button>
      <span>{{currentPage}} of {{lastPage = Math.round(photos.length / pageSize)}}</span>
      <button class="button" @click="nextPage">Next</button>
      <button class="button" @click="seeAll">See all</button> 
    </p>
  <table>
    <thead>
      <tr>
        <th @click="sort('id')"> Id  <i class="fas fa-chevron-down"></i> </th>
        <th @click="sort('title')">Title  <i class="fas fa-chevron-down"></i></th>
        <th @click="sort('url')">Url  <i class="fas fa-chevron-down"></i></th>
        <th @click="sort('albumId')">album_Id <i class="fas fa-chevron-down"></i></th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="(photo, i) in sortedPhotos" :key="i">
        <td>{{photo.id}}</td>
        <td>{{photo.title}}</td>
        <td>{{photo.url}}</td>
        <td>{{photo.albumId}}</td>
      </tr>
    </tbody>
  </table>
      <p>
      <button class="button" @click="prevPage">Previous</button>
      <span>{{currentPage}} of {{lastPage = Math.round(photos.length / pageSize)}}</span>
      <button class="button" @click="nextPage">Next</button> 
      <button class="button" @click="seeAll">See all</button> 
    </p>  
</div>
</template>
<script>
export default {
  name: "app",
  data(){
    return {
        photos:[],
        currentSort:'name',
        currentSortDir:'asc',
        pageSize: 20,
        ascending: false,
        loading:false, 
        currentPage:1,
        lastPage: Number
        
    
    }
  },
  created:function() {
    fetch('https://jsonplaceholder.typicode.com/photos')
    .then(res => res.json())
    .then(res => {
      this.photos = res;
    })
  },
  methods:{
    sort:function(s) {
      if(s === this.currentSort) {
        this.currentSortDir = this.currentSortDir==='asc'?'desc':'asc';
      }
      this.currentSort = s;
    },
    nextPage:function() {
       if((this.currentPage*this.pageSize) < this.photos.length){
         return this.currentPage++
       } 
    },
    prevPage:function() {
       if(this.currentPage > 1){
         return this.currentPage--
       }
    },
    seeAll:function(){
    return
      this.pageSize = 5
   }
  },
  computed:{
    sortedPhotos:function() {
      return this.photos.sort((a,b) => {
        let modifier = 1;
        if(this.currentSortDir === 'desc') modifier = -1;
        if(a[this.currentSort] < b[this.currentSort]) return -1 * modifier;
        if(a[this.currentSort] > b[this.currentSort]) return 1 * modifier;
        return 0;
      }).filter((row, index) => {
        let start = (this.currentPage-1)*this.pageSize;
        let end = this.currentPage*this.pageSize;
        
        if(index >= start && index < end ) return true;
    });
  }
  }
}
  

</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin: 0 auto;

}
span {
    display: inline-block;
    padding: 9px 10px;
    border: solid 2px white;
    background: black;
    font-size: .875em;
    font-weight: bold;
    text-decoration: none;
    color: white;
}
table th {
  text-transform: uppercase;
  text-align: center;
  color: #FFF;
  cursor: pointer;
  padding: 9px;
  min-width: 55px;
}

table th:hover {
        background: #717699;
      }
table td {
  width: 10vw;
  text-align: center;
  padding: 8px;
  height: 5vh;
}
table td:last-child {
  border-right: none;
}

table {
  font-size: 15px;
  align-items: center;
  width: 700px;
  margin: 0 auto;
  border-collapse: collapse;
  padding: 20px;
  transition: all 0.2s linear;
  cursor: pointer;
  box-shadow: 0 3px 12px 0 rgba(0, 0, 0, 0.2), 0 1px 15px 0 rgba(0, 0, 0, 0.19);
}
table:hover{
  transform: scale(1.01);
  
}

table th {
 
  text-align: left;
  background: black;
  color: #FFF;
  cursor: pointer;
  padding: 8px;
  min-width: 30px;
  border-right: 2px solid white;
  
}
table th:last-child{
  border-right: none;
}
table th:hover {
  background: rgba(0, 0, 0, 0.92);
  color: white;
}
table td {
  text-align: left;
  padding: 8px;
  
}
table td:last-child {
  border-right: none;
}
.button {
      display: inline-flex;
      align-items: center;
      justify-content: space-between;
      height: 34px;
      padding: 0 17px;
      cursor: pointer;
      background: transparent;
      border: none;
      border-radius: 0;
      text-align: center;
      font-weight: 600;
      font-size: 12px;
      text-transform: uppercase;
      white-space: nowrap;
     
    }
    button:hover{
        background: rgba(0, 0, 0, 0.92);
        color: white;
    }
    .button:focus {
      outline: 0;
    }
    .button {
      background: black;
      color: #ffffff;
    }
    .button + .button {
      margin-left: 1em;
    }
    .button.-fill-gray {
      background: rgba(0, 0, 0, 0.5);
      color: #ffffff;
    }
    .button.-size-small {
      height: 32px;
    }
    .button.-icon-right {
      text-align: left;
      padding: 0 20px;
    }
    .button.-icon-right > .icon {
      margin-left: 10px;
    }
    .button.-icon-left {
      text-align: right;
      padding: 0 20px;
    }
    .button.-icon-left > .icon {
      margin-right: 10px;
    }
    .button.-icon-center {
      padding: 0 20px;
    }

</style>
