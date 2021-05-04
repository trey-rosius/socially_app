<template>
     <div class="container">
    <section class="title">
        <div class="app-name">Socially</div>
    </section>
    <div class="content">

        <input type="file" @change="onFileChange" />
  
      <div id="preview">
    <img class= "postpic" v-if="url" :src="url" />
  </div>
        <textarea v-model="postText" class="content__post" rows="4" cols="10" placeholder="write something ....."/><br>
       
       
        <input type="submit" value="Add Post" class="btn" @click="addPost">

        
      </div>
   </div>
   
</template>

<script>
import add_image from '@/assets/add_pic.svg';
    export default {
        data(){
            return {
               postText:String,
              url:add_image,
               filename:String,
           filePath:String,
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
     addPost(e) {
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
.container{
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  
  height: 100vh;

  width: 100%;
  
 
}
</style>