<template>
    <div class="update-profile">

        <div class="name">Socially</div>
  
    <div class="content">
 <input type="file" @change="onFileChange" />
  
      <div id="preview">
    <img class= "profilepic" v-if="url" :src="url" />
  </div>
       
        <input type="submit" value="Upload Image" class="btn" @click="upload">

        
      </div>
   </div>
</template>

<script>
import { API } from 'aws-amplify';
import { Storage }  from 'aws-amplify';
import { v4 as uuidv4 } from 'uuid';
import profile_icon from '@/assets/profile.png';

import {createUser} from '../src/graphql/mutations';
    export default {
        mounted(){
            this.first_name = this.username;
            this.email = this.email;
            console.log(this.email);
         

            

        },
    
       props:{
           username:String,
           email:String

       },
       data(){
           return{
               first_name:String,
           url:profile_icon,
           filename:String,
           filePath:String,
           }

       },
       methods:{
           
           
 async addUserToDb(){
                const uuid = uuidv4();
               const {username,email,filename} = this;
               const user = {id:uuid,username,email,profilePicUrl:filename};
               await API.graphql({
                   query:createUser,
                   variables:{input:user},
               }).then((result) =>{
                   console.log("result is"+result);
               })
               console.log("successfully uploaded");
               this.$router.push({name:'home',params:{id: uuid}});
           },
          
           onFileChange(e) {
      const file = e.target.files[0];
      console.log(file);
      this.filename =file.name;
      this.filePath = file; 
      
      this.url = URL.createObjectURL(file);
    },
    upload(e) {
        console.log(this.filename);
       const result = Storage.put(this.filename, this.filePath, {
          
           contentType:this.filePath.type,
    progressCallback(progress) {
        if(`${progress.loaded}` === `${progress.total}` ){
          
         
      
        }
        console.log(`Uploaded: ${progress.loaded}/${progress.total}`);
  },
}).then((result) =>{

    this.addUserToDb();
})
    }

         
       }
      
    }
</script>

<style lang="scss" scoped>
.name{
    background-image: linear-gradient(120deg, var(--color-grad-1) 0%, var(--color-grad-2) 100%);
    font-family: var(--app-name-font);
    font-size: 60px;
    margin-top: 20px;
    height: 100px;
   
    font-weight: bold;
    
    
    -webkit-background-clip: text;
    background-clip: text;
    -webkit-text-fill-color: transparent;
}
.update-profile{
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
}
.content{
    margin-top: 10px;
   
}
input[type="file"]{
    margin-top: 20px;
    margin-bottom: 20px;
}

</style>