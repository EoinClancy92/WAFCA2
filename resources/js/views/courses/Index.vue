<template>
  <b-row align-h="center">
    <b-col cols="12">

      <div class="card">
      <b-table-simple responsive>
          <div class="card-header">
        <b-head>
          <b-tr>
            <b-th>
              Title
            </b-th>
            <b-th>
              Code
            </b-th>
            <b-th>
              Description
            </b-th>
            <b-th>
              Points
            </b-th>
            <b-th>
              Level
            </b-th>
            <b-th>
              Actions
            </b-th>
          </b-tr>
        </b-head>
      </div>
      <div class="card-body">
        <b-body>
          <b-tr v-for="item in items" :key="item.id">
            <b-td>{{item.title}}</b-td>
            <b-td>{{item.code}}</b-td>
            <b-td>{{item.description}}</b-td>
            <b-td>{{item.points}}</b-td>
            <b-td>{{item.level}}</b-td>
            <b-td>
              <b-button class="submit">
              <router-link :to="`/courses/show/${item.id}`">
              View
              </router-link>
              </b-button>
            </b-td>
            <b-td>
              <b-button class="submit">
              <router-link :to="`/courses/edit/${item.id}`">
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
        </b-body>
      </div>
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
    axios.get('/api/courses',{
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
        axios.delete('/api/courses/'+ item.id,{
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

        axios.delete('/api/courses/'+ item.id,{
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
