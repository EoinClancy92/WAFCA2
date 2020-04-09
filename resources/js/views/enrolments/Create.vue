<template>
  <b-row>
    <b-col cols="8">
      <div class="card" tag="article">
        <div class="card-header">
          <h1>Create Enrolment</h1>
        </div>
        <div class="card-body">
        <b-form @submit="onSubmit">
            <b-form-group id="input-group-1"
             label="Date:"
             label-for="input-1">
             <b-form-input
             id="input-1"
             type="date"
             required
             placeholder="Enter Date:"
             v-model="form.date"
             >

             </b-form-input>

            </b-form-group>

            <b-form-group id="input-group-2"
             label="Time:"
             label-for="input-2">
             <b-form-input
             id="input-2"
             type="time"
             required
             placeholder="Enter Time"
             v-model="form.time"
             >

            </b-form-input>
            </b-form-group>
            <td>Status</td><br>
            <select class="form-control" @change="changeStatus($event)" v-model="status">
                <option value="" disabled selected>Select Status</option>
                <option value="career_break">career_break</option>
                <option value="assigned">assigned</option>
                <option value="interested">interested</option>
                <option value="associate">associate</option>
            </select><br>

            <td>Course</td></br>
            <td>
              <select class="form-control" @change="changeCourse($event)" v-model="course">
                <option value="" disabled selected>Select Course</option>
                <option v-for='course in courses' :value='course.id' :key="course.id">{{ course.title }}</option>
              </select>
            </td>
            <br>


            <td>Lecturer</td><br>
                <td>
                  <select class="form-control" @change="changeLecturer($event)" v-model="lecturer">
                    <option v-for='lecturer in lecturers' :value='lecturer.id' :key="lecturer.id">{{ lecturer.name }}</option>
                  </select>
                </td>
              </br>
        <b-button type="submit" class="submit">Submit</b-button>
        </b-form>
      </div>
    </div>
    </b-col>
  </b-row>
</template>

<script>
export default {
  data(){
    return {
        courses:[],
        lecturers:[],
        selectedCourse:null,
        selectedStatus:null,
        selectedLecturer:null,
        form:{
          date: '',
          time: '',
          status: this.selectedStatus,
          course: this.selectedCourse,
          lecturer: this.selectedLecturer
      },
      loggedIn: false,
      errors:[]
    }
  },
  created(){
    if(localStorage.getItem('token')){
      this.loggedIn = true;
    }else{
      this.loggedIn = false;
      this.$router.push('/');
    }

    let app = this;
    let token = localStorage.getItem('token');
    axios.get('/api/courses',{
      headers: {Authorization: "Bearer " + token}
    })
    .then(function (response) {
      console.log(response.data.data);
       app.courses = response.data.data;
    })
    .catch(function (error) {
       console.log(error);
    });
    axios.get('/api/lecturers',{
      headers: {Authorization: "Bearer " + token}
    })
    .then(function (response) {
       app.lecturers = response.data.data;
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

      axios.post('/api/enrolments/', {
        date: app.form.date,
        time: app.form.time,
        status: app.seletedStatus,
        course_id: app.selectedCourse,
        lecturer_id: app.selectedLecturer
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
      this.selectedCourse = event.target.options[event.target.options.selectedIndex].value
    },
    changeLecturer(event){
      this.selectedLecturer = event.target.options[event.target.options.selectedIndex].value
    },
    changeStatus(event){
      this.selectedStatus = event.target.options[event.target.options.selectedIndex].value
    }

  }
}
</script>
