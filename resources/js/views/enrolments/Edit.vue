<template>
  <b-row align-h="center">
    <b-col cols="8">
      <h3 v-if="!loggedIn">You are not logged in!!</h3>
      <div class = "card"
        v-else
        tag="article"
      >

      <div class="card-header">
        <h1>Edit Enrolment</h1>
      </div>

      <div class="card-body">
        <b-form @submit="onSubmit">
          <b-form-group
            id="input-group-1"
            label="Date:"
            label-for="input-1"
          >
            <b-form-input
              id="input-1"
              v-model="enrolment.date"
              type="date"
              required
              placeholder="Enter date"

            ></b-form-input>
          </b-form-group>

          <b-form-group
            id="input-group-2"
            label="time:"
            label-for="input-2"
          >
            <b-form-input
              id="input-2"
              v-model="enrolment.time"
              type="time"
              required
              placeholder="Enter time"

            ></b-form-input>
          </b-form-group>



          <td>Status</td></br>
          <select class="form-control" @change="changeStatus($event)" v-model="enrolment.status">
              <option selected>select</option>
              <option value="career_break">career_break</option>
              <option value="assigned">assigned</option>
              <option value="interested">interested</option>
              <option value="associate">associate</option>
          </select>



          <td>Course</td></br>
          <select class="form-control" @change="changeCourse($event)" v-model="enrolment.course_id">
              <option selected>{{enrolment.course.title}}</option>
              <option v-for='course in courses' :value='course.id' :key="course.id">{{ course.title }}</option>
          </select>




          <td>Lecturer</td></br>
          <select class="form-control" @change="changeLecturer($event)" v-model="enrolment.lecturer_name">
              <option selected>{{enrolment.lecturer.name}}</option>
              <option v-for='lecturer in lecturers'  :value='lecturer.id' :key="lecturer.id">{{ lecturer.name }}</option>
          </select>
        </br>




          <b-button class="submit" type="submit">Submit</b-button>
        </b-form>
      </b-card>
    </div>
  </div>
  </b-col>
  </b-row>
</template>



<script>
  export default {
    data() {
      return {
        enrolment: {},
        courses:{},
        lecturers:{},

        show: true,
        loggedIn: false
      }
    },
    created() {
      // console.log(localStorage.getItem('token'));
      if (localStorage.getItem('token')) {
        this.loggedIn = true;
      }
      else {
        this.loggedIn = false;
      }

      let app = this;
      let token = localStorage.getItem('token');
      axios.get(`/api/enrolments/${app.$route.params.id}`, {
        headers: { Authorization: "Bearer " + token }
      })
      .then(function (response) {
        app.enrolment = response.data.data;
        console.log(app.enrolment);
      })
      .catch(function (error) {
        console.log(error);
      });
      axios.get(`/api/courses/`, {
        headers: { Authorization: "Bearer " + token }
      })
      .then(function (response) {
        app.courses = response.data.data;
        console.log(app.courses);
      })
      .catch(function (error) {
        console.log(error);
      });
      axios.get(`/api/lecturers/`, {
        headers: { Authorization: "Bearer " + token }
      })
      .then(function (response) {
        app.lecturers = response.data.data;
        console.log(app.lecturers);
      })
      .catch(function (error) {
        console.log(error);
      });
    },
        methods:{
          onSubmit(evt){
            evt.preventDefault()
            let app = this;
            let token = localStorage.getItem('token');
            console.log(app.form);
            axios.put(`/api/enrolments/${app.$route.params.id}`, {
              date: app.enrolment.date,
              time: app.enrolment.time,
              status: app.enrolment.status,
              course_id: app.enrolment.course_id,
              lecturer_id: app.enrolment.lecturer_id
             },
             {
              headers: { Authorization: `Bearer ${token}` }
            })
            .then(function(response) {
              app.$router.push('/enrolments');
            })
            .catch(function(error){
              console.log(error.response.data);
              app.errors = error.response.data.errors;
            })
          },
          changeCourse(event){
              this.selectedCourse = event.target.options[event.target.options.selectedIndex].text
              console.log("Course: "+this.selectedCourse);
              console.log("date: "+this.form.date);
            },
            changeStatus(event){
                this.selectedStatus = event.target.options[event.target.options.selectedIndex].value
                console.log("status: "+this.selectedStatus);
            },
            changeLecturer(event){
                this.selectedLecturer = event.target.options[event.target.options.selectedIndex].text
                console.log("Lecturer : "+this.selectedLecturer);
            }
        }



  }
</script>
