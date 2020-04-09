<template>
  <b-row align-h="center">
    <b-col cols="8">
      <h3 v-if="!loggedIn">You are not logged in!!</h3>
      <div class = "card"
        v-else
        tag="article"
      >
      <div class="card-header">
        <h1>Course</h1>
      </div>
      <div class="card-body">
            <h3>Title: {{course.title}}</h3><br>
            <h3>Code: {{course.code}}</h3><br>
            <h3>Description: {{course.description}}</h3><br>
            <h3>Points: {{course.points}}</h3><br>
            <h3>Level: {{course.level}}</h3>
        </div>
      </div>
      </b-card>
    </b-col>
  </b-row>
</template>



<script>
  export default {
    data() {
      return {
        course: {},
        show: true,
        loggedIn: false
      }
    },
    created() {
      if (localStorage.getItem('token')) {
        this.loggedIn = true;
      }
      else {
        this.loggedIn = false;
      }



      let app = this;
      let token = localStorage.getItem('token');
      axios.get(`/api/courses/${app.$route.params.id}`, {
        headers: { Authorization: "Bearer " + token }
      })
      .then(function (response) {
        app.course = response.data.data;
        console.log(app.enrolment);
      })
      .catch(function (error) {
        console.log(error);
      });
    },
        methods:{



        }



  }
</script>
