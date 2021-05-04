<template>
    <div class="update-profile">

        <div class="name">Socially</div>
  
    <div class="content">
 <input type="file" @change="onFileChange" />
  
      <div id="preview">
    <img v-if="url" :src="url" />
  </div>
       
        <input type="submit" value="Upload Image" class="btn" @click="upload">

        
      </div>
   </div>
</template>

<script>
import { Storage }  from 'aws-amplify';
import profile_icon from '@/assets/profile.png';
    export default {
        mounted(){
            this.first_name = this.username;
            this.email = this.email;
            

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
           filePath:String
           }

       },
       methods:{
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
        console.log(`Uploaded: ${progress.loaded}/${progress.total}`);
  },
});
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
#preview {

  display: flex;
  justify-content: center;
  align-items: center;
}

#preview img {
  width: 200px;
  height: 200px;
  margin: 10px;
  border-radius: 100%;
  object-fit: cover;
}
</style>