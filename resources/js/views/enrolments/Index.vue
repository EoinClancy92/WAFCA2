<template>
  <b-row align-h="center">
    <b-col cols="12">

      <div class="card">
      <b-table-simple responsive>
          <div class="card-header">
          <b-tr>
            <b-th>
              Date
            </b-th>
            <b-th>
              Time
            </b-th>
            <b-th>
              Status
            </b-th>
            <b-th>
              Course
            </b-th>
            <b-th>
              Lecturer
            </b-th>
            <b-th>
              Actions
            </b-th>
          </b-tr>
      </div>

      <div class="card-body">
          <b-tr v-for="item in items" :key="item.id">
            <b-td>{{item.date}}</b-td>
            <b-td>{{item.time}}</b-td>
            <b-td>{{item.status}}</b-td>
            <b-td>{{item.course.title}}</b-td>
            <b-td>{{item.lecturer.name}}</b-td>
            <b-td>
              <b-button class="submit">
              <router-link :to="`/enrolments/edit/${item.id}`">
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

        axios.delete('/api/enrolments/' + item.id,{
        headers: {Authorization: "Bearer "+ token}

      }).then(function (response){
        let itemIndex = app.items.findIndex(function(obj){return obj.id = item.id})
        app.items.splice(itemIndex, 1);
      });
    }
  }
}
</script>

<style>

</style>
