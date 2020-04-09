<template>
  <div>
    <myNavbar />
    <b-container>
      <router-view />
    </b-container>
  </div>
</template>
<script>

import MyNavbar from './components/myNavbar'

export default {
  name: 'app',
  components: {
    MyNavbar
  },
  data() {
    return {

    }
  }
}
</script>
<style>

body {
   background-color: #a6a6a6!important;
}

.card{
  margin-top:1em;
  border-radius:10px!important;
  background-color: #a6a6a6!important;
  overflow:hidden;
 /* border-radius: 20px; */
 /* background-color: #a6a6a6; */

}
.table{
  color: white!important;
}
.item{
  color:black;
}
.card-header {
  border-radius: 15px!important 15px!important 0px!important 0px!important;
   color: white;
   background-color: #343a40!important;
   overflow:hidden;


  /* text-align: center; */

}
.card-question{
  background-color:#343a40;
  border-radius: 10px;
  overflow:hidden!important;
}
.card-body  {
     align-items: center;
     color: white;
     background-color: #656569;
     border-radius: 0px 0px 10px 10px;
     overflow:hidden!important;

}
.drop-item{
  color:black!important;
  background-color: #343a40!important;
}
.link{
  color: white;
}

.table{
  color: white;
}

.submit{
  background-color: #25235e!important;
  color: white!important;
  align-items: center;

}

.cancel{
  background-color: red;
  align-items: center;
  color:white;
}
a { color: white!important;
text-decoration:none;
}
a:hover {
color:#000000!important;
text-decoration : none!important;
}

</style>
