<template>
  <div class="collect-photo">
    <div class="photo-list" v-for="photo in photoList" v-show="photoList">
      <p>{{photo.time}}</p>
      <router-link :to="'/photo/photo_collect_detail/'+img.p_id" v-for="img in photo.photo">
        <img :src="img.p_src" alt=""> 
      </router-link>
    </div>
    <div class="nothing" v-show="!photoList">
        <p>收藏夹怎么是空的Σ(っ°Д°;)っ</p>
    </div>
  </div>
</template>

<script>

  import Axios from 'axios'
  import $ from 'jquery'
  export default {
  data() {
    return {
      photoList:[],
      // show:true,
      login_id:0,
    }
  },
  mounted:function() {
    if(document.cookie){
      var arr=document.cookie.split(";")[1];
      var new_arr=arr.split("=")[1];
      this.login_id=new_arr;
      this.collect_photo_all();
    } 
  },
  methods:{
    collect_photo_all:function(){
      Axios.get("http://localhost:3000/collect_photo_all",{
        params:{
          u_id:this.login_id,
        }
      }).then((res)=>{
        this.photoList=JSON.parse(res.data);
        // console.log(this.photoList);
        // console.log(JSON.parse(res.data));
      }).catch((error)=>{
          console.log(error);
      });
    }
  }
}

</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

  @import "../../assets/css/reset.css";
  .collect-photo{
    margin-top: 1.63rem;
  }
  .photo-list{
    padding: .1rem .4rem .4rem;
    /*margin-top: 0.4rem;*/
  }
  
  .nothing{
    text-align: center;
    font-size: .3rem; 
    margin-top: 2rem;
  }
  .photo-list p{
    color:#888;
  }
  img{
    margin: .1rem;
    width: 1.6rem;
    height: 1.6rem;
  }



</style>
