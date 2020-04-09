<template>
  <b-row align-h="center">
    <b-col cols="12">

      <div class="card">
      <b-table-simple responsive>
          <div class="card-header">
          <b-tr>
            <b-th>
              Name
            </b-th>
            <b-th>
              Address
            </b-th>
            <b-th>
              Email
            </b-th>
            <b-th>
              Phone
            </b-th>
            <b-th>
              Actions
            </b-th>
          </b-tr>
      </div>
      <body>
      <div class="card-body">
          <b-tr v-for="item in items" :key="item.id">
            <b-td>{{item.name}}</b-td>
            <b-td>{{item.address}}</b-td>
            <b-td>{{item.email}}</b-td>
            <b-td>{{item.phone}}</b-td>
            <b-td>
              <b-button class="submit">
              <router-link :to="`/lecturers/edit/${item.id}`">
              Edit
              </router-link>
              </b-button>
            </b-td>
            <b-td>
              <b-button class="cancel" @click="deleteData(item)">
                Delete
              </b-button>
            </b-td>
          </b-tr>
        </div>
      </body>
      </b-table-simple>
    </div>
    </b-col>
  </b-row>
</template>

<script>
export default{
  data(){
    return {
      items: []
    }
  },
  created(){
    let app = this;
    let token = localStorage.getItem('token');

    axios.get('/api/enrolments',{
      headers: {Authorization: "Bearer " + token}
    })
    .then(function (response) {
       console.log(response.data.data);
       app.enrolments = response.data.data;
    })
    .catch(function (error) {
       console.log(error);
    });

    axios.get('/api/lecturers',{
      headers: {Authorization: "Bearer " + token}
    })
    .then(function (response) {
       console.log(response.data.data);
       app.items = response.data.data;
    })
    .catch(function (error) {
       console.log(error);
    });
  },
  methods: {
    deleteData(item){

      let app = this;
      let token = localStorage.getItem('token');
      if(item.enrolments.length > 1){
        alert("Delete all Enrolments first");
      }else if(item.enrolments.length === 0){
        axios.delete('/api/lecturers/'+ item.id,{
          headers: {Authorization: "Bearer "+ token}
        })
        .then(function (response){
          let itemIndex = app.items.findIndex(function(obj){return obj.id == item.id})
          app.items.splice(itemIndex, 1);
        });
      }else{
        axios.delete('/api/enrolments/' + item.enrolments[0].id,{
        headers: {Authorization: "Bearer "+ token}

      }).then(function (response){

        axios.delete('/api/lecturers/'+ item.id,{
          headers: {Authorization: "Bearer "+ token}
        })
        .then(function (response){
          let itemIndex = app.items.findIndex(function(obj){return obj.id == item.id})
          app.items.splice(itemIndex, 1);
        });
      })
    }
   }
  }
}
</script>

<style>

</style>
