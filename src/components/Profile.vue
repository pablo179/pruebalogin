<template>
    <div class="back">
      <div class="box">
            <div class="title">Bienvenido {{profile.name.username}}</div>
              <svg >
                <symbol id="s-text">
                  <text text-anchor="middle"
                        x="50%"
                        y="50%"
                        class="text--line"
                        >
                    Woonkly
                  </text>
                </symbol>
  <g class="g-ants">
    <use xlink:href="#s-text"
      class="text-copy"></use>
    <use xlink:href="#s-text"
      class="text-copy"></use>
    <use xlink:href="#s-text"
      class="text-copy"></use>
    <use xlink:href="#s-text"
      class="text-copy"></use>
    <use xlink:href="#s-text"
      class="text-copy"></use>
  </g>
</svg>
      <span class="signout">
          <button @click="getout"> Sign Out</button>
      </span>
      </div>
    </div>
</template>

<script>
export default {
  name: 'Profile',
  created: function getuser(){
    const userToken = JSON.parse(localStorage.getItem('userToken'));
    let url ='https://loginpablo.herokuapp.com/my_user/';
        var request = new Request(url,{
          method: 'GET',
          headers: new Headers({
            'Authorization':'Token '+userToken,
            'Content-Type': 'application/json'
          })
        });
        fetch(request)
            .then(r=>r.json())
            .then(data=>{
                this.profile=data
             })
            .catch(e=>{
                console.log(e)
        })
  }, 
  methods: {
    getout () {
    localStorage.removeItem('userToken');
      this.$router.push('/')
    }
  },
  data(){
    return{
      profile:{
        name:{
        username:''
        }
      }
    }
  }
}
</script>
<style scoped>
@import url('login.css');
.title{
    font-size: 2.8em;
}
  .box{
    margin-top: 4.5%;
    width: 550px;
    display: block;
    height: auto;
  }
  .signout{
    margin-left: 28%;
    z-index: 3;
  }
  .signout button{
      width: 40%;
      margin: 10px auto;
      z-index: 4;
      cursor: pointer;
      height: 40px;
      border: none;
      background: rgb(60, 60, 61);
      border-radius: 50px;
      color: white;
      font-size: 1.2em;
      outline-style: none;
      transition: all .4s ease-in-out;
  }

</style>
