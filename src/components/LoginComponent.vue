<template>
    <div class="back" v-if="havecount">
        <div class="cover"></div>
        <form v-on:submit.prevent="onSubmit" id="forms">
            <div class="box">
                <div class="title">Sign In</div>
                <div class="boxes">
                    <span class="line">
                        <div class="profile icon"></div>
                        <input type="text" required v-model="user.username">
                        <span id="userinp" class="txt">Username</span>
                    </span>
                    <span class="line">
                        <div class="key icon"></div>
                        <input type="password" required v-model="user.password" name="passinp">
                        <span id="pasinp" class="txt">Password</span>
                    </span>
                    <span class="signin">
                        <button   type="submit" id="sigbut"> <div class="pal"></div> Sign In</button>
                    </span>
                    <span class="signup">
                        ¿no tienes una cuenta? <span @click="change" class="lisi">Sign Up</span>
                    </span>
                </div>
            </div>
        </form>
    </div>
        <div class="back" v-else>
        <div class="cover"></div>
        <form v-on:submit.prevent="onSubmitup" id="forms">
        <div class="box">
            <div class="title">Sign Up</div>
            <div class="boxes">
                <span class="line">
                    <div class="profile icon"></div>
                    <input type="text" required v-model="newuser.username">
                    <span class="txt">Username</span>
                </span>
                <span class="line">
                    <div class="key icon"></div>
                    <input  type="password" required v-model="newuser.password">
                    <span id="pasinp" class="txt">Password</span>
                </span>
                <span class="line">
                    <div class="key icon"></div>
                    <input  type="password" required v-model="newuser.confirm">
                    <span id="confinp" class="txt">Confirm Pass</span>
                </span>
                <span class="signin">
                    <button type="submit" id="sigbut"> <div class="pal"></div> Sign Up</button>
                </span>
                <span class="signup">
                    ¿ya tienes una cuenta? <span  @click="change" class="lisi">Sign In</span>
                </span>
            </div>
        </div>
        </form>
    </div>
</template>
<style>
@import url('https://fonts.googleapis.com/css?family=Telex');
@import url('login.css');
#sigbut.error{
    font-size: 1.5em;
    width: 60%;
    height:40px;
    transform: rotate(0deg);
    transition: ease-in all .1s;
}
#sigbut.error>.pal{
    display: none;
}
</style>
<script>
export default {
  name: 'Logincomponent',
  mounted: function checklog(){
    let userToken=JSON.parse(localStorage.getItem('userToken'));
    if(userToken){
          this.$router.push('/profile')
    }
  }, 
  data () {
    return {
      havecount: true,
        user: {
        username: '',
        password: ''
      },
      newuser:{
        username: '',
        password: '',
        confirm:''
      }
    }
  },
  methods: {
    change () {
      this.havecount = !this.havecount
    },
    onSubmit () {
        var loc =this.user;   
        var rut =this.$router;
        document.getElementById('sigbut').classList.remove('error');
    document.getElementById('pasinp').classList.remove('errortxt');
    document.getElementById('userinp').classList.remove('errortxt');
    document.getElementById('sigbut').classList.add('action');
        window.setTimeout( function(){
      let url = 'https://loginpablo.herokuapp.com/api-token-auth/'
      var request = new Request(url, {
        method: 'POST',
        body: JSON.stringify(loc),
        headers: new Headers({
          'Content-Type': 'application/json'
        })
      })
      fetch(request)
        .then(r => {
          if (r.ok) return r.json()
        })
        .then(data => {
          localStorage.setItem('userToken', JSON.stringify(data.token))
          rut.push('/profile')
        }).catch(e => {
          // eslint-disable-next-line
        document.getElementById('sigbut').classList.remove('action');
            document.getElementById('sigbut').classList.add('error');
            document.getElementById('pasinp').classList.add('errortxt');
            document.getElementById('userinp').classList.add('errortxt');
        })
        },750,this.loc,this.rut)
    },
    onSubmitup () {
        if(this.newuser.password==this.newuser.confirm){
        var loc =this.newuser;   
        var rut =this.$router;
                document.getElementById('sigbut').classList.remove('error');
                document.getElementById('pasinp').classList.remove('errortxt');
                document.getElementById('confinp').classList.remove('errortxt');
    document.getElementById('sigbut').classList.add('action');
      
        window.setTimeout( function(){

      let url = 'https://loginpablo.herokuapp.com/users/';
        console.log(loc)
        var request = new Request(url, {
            method: 'POST',
            body: JSON.stringify(loc),
            headers: new Headers({
                'Content-Type': 'application/json'
            })
            });
            fetch(request)
            .then(r => {
            if (r.ok) return r.json()
            })
            .then(data => {
                location.reload();
            }).catch(e => {
                window.alert("hubo un error: "+e)
            }) 

        },750,this.loc,this.rut)

            }
            else{
        document.getElementById('sigbut').classList.remove('action');
                document.getElementById('sigbut').classList.add('error');
                document.getElementById('pasinp').classList.add('errortxt');
                document.getElementById('confinp').classList.add('errortxt');
            }
     }
    }
}
</script>
