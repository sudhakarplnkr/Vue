<template>
  <div class="hello">
 <ul>
  <li v-for="employee in employees" v-bind:key="employee.name">
    {{ employee.name }}
  </li>
</ul>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },
  data:function(){
    return {
      employees: []
    }
  },
   mounted() {
     const {$axios} = this;
     $axios.post('http://localhost:7000/authentication-api/login',{
"Username": "John",
"Password": "nosecret"
}).then((userData)=>{
const options = {
      method: 'GET',
      headers: {
        'content-type': 'application/json',
        'Authorization': 'Bearer '+userData.data.token,
      },
      url: 'http://localhost:7000/employee-api/employees',
    };
      $axios(options)
      .then((response) => {
        this.employees = response.data;
      });
     });
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: upper-roman;
  padding: 0;
}
li {
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
