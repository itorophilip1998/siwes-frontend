<template>
  <div>
    <Header :info="info"/>
    <Loader v-if="loader" />

      <div class="databox pb-3 bg-light">
        <form class="mt-5 py-2 p-3" @submit.prevent="upload()">
              <div class="alert alert-danger" v-if="err" role="alert">
              
                <small>  Ops, can't right now,error</small>
              </div>
    <!-- <div class="add text-muted  w-100 text-center"><span class=" bg-light ">Additional information</span></div> -->

            <input type="text" v-model="details.department" required class="w-100 mt-3" placeholder="Department">

            <input type="text" v-model="details.school" required class="w-100 mt-4" placeholder="School">

               <select name="" id="" v-model="details.firm_type" required class="w-100 mt-4" >
                 <option value="" >Select Type of  Firm </option>
                 <option  selected v-for="firm in firms" :key="firm">{{ firm }}</option>
               </select>
               <select name="" id="" v-model="details.duration" required class="w-100 mt-4" >
                 <option value="" selected>Select Location</option>
                 <option >1-year IT</option>
                 <option >4-month Siwes</option>
               </select>

            <button class="btn-success shadow w-100 mt-5 btn rounded-pill p-2" >Upload Document</button>

     </form>
      </div>
  </div>
</template>


<script>
import Header from '@/components/auth-header';
import EasyCamera from 'easy-vue-camera';
// import { WebCam } from "vue-web-cam";
import Loader from './Loader'

export default {
  middleware:'auth',

  components:{
    Header,
    'v-easy-camera': EasyCamera,
   
     Loader,
    //  'vue-web-cam': WebCam,
  },
  data() {
    return {
      firms:[
        'Hopital',
        'School',
        'Innovation Hub',
        'Church',
        'Petrolium Company',
        'Ministry(Government)',
        'Private Company',
      ],
      isSnap:false,
    passwordCheckData:false,
    picture:"",
      info:{
          name:"Document",
          short_name:"Let Know More about you!",
          details:"Let the firm know more about you",
          icon:"fa-book",
          dashboard:false,
      },
       details:{
    department: "Computer Science Department",
    school: "Applied",
    firm_type: "",
    duration: "",
    user_id: "",
  },
      loader: false,
      err:false

    }
  },
  
  methods: {
    upload(){ 

          const token = localStorage.getItem('token')
      this.details.user_id = localStorage.getItem('user_id')

      this.loader = true
      const config = {
        headers: {
          Authorization: `Bearer ${token}`,
        },
      }
          this.$axios.post('/upload-document',this.details,config).then((result) => {
          this.loader=false
          localStorage.setItem('dept',result.data.result.department)
          localStorage.setItem('sch',result.data.result.school) 
            this.$router.push("/dashboard")
          }).catch((err) => {
          this.loader=false
            this.err=!this.err

          setTimeout(()=>{
            this.err=!this.err
          }, 3000)
            this.err=!this.err
          
          });
        },
    isSnappFuc(){
            this.isSnap=!this.isSnap;
            console.log(`Clicked and set ${this.isSnap}`)
    },
    output(){
            console.log(`Clicked and set ${this.picture}`)

    },
   passwordCheck(data){
             if (data=="show") {
                 this.passwordCheckData=!this.passwordCheckData;
                 this.$refs.password.type="text"
             }
             else{
                this.passwordCheckData=!this.passwordCheckData;
                this.$refs.password.type="password"

             }
        }
},

}
</script>
<style >
.camera{
  border:2px dashed var(--success);
  border-radius:5px;

}
.add{
  display: absolute !important;
  margin-top: -32px;

}
i.fa-eye-slash,i.fa-eye{
  position:absolute ;
  right:30px;
  margin-top:-32px;
}

</style>

