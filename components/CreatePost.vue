<template>
<div class="container">
    
    <transition name="modal">
        <div class="modal-mask">
          <div class="modal-wrapper">
            <div class="modal-container">

              <div class="modal-header">
             
        <div class="app-name">Socially</div>
  
              </div>

              <div class="modal-body">
                <div class="content">
   
        <input type="file" @change="onFileChange" />
  
      <div id="preview">
    <img class= "postpic" v-if="url" :src="url" />
  </div>
        <textarea v-model="postText" class="content__post" rows="4" cols="10"/><br>
       
       
        <input type="submit" value="Add Post" class="btn" @click="addPost">

        
      </div>
              </div>

              
            </div>
          </div>
        </div>
      </transition>
</div>

</template>

<script>
import add_image from '@/assets/add_pic.svg';
import { API } from 'aws-amplify';
import { Storage }  from 'aws-amplify';
import { v4 as uuidv4 } from 'uuid';
import {createPost} from '../src/graphql/mutations';
    export default {
        props:{
           userId:String

       },
        data(){
            return {
               postText:undefined,
              url:add_image,
               filename:String,
           filePath:String,
            }
        },
        methods:{

           async addPostToDb(){
                const uuid = uuidv4();
               const {userId,postText,filename} = this;
               const signedURL = await Storage.get(filename); 
               const post = {id:uuid,userID:userId,postText,status:"CREATED",postImageUrl:signedURL};
               
               console.log("signed url"+signedURL);
               await API.graphql({
                   query:createPost,
                   variables:{input:post},
               }).then((result) =>{
                   console.log("result is"+result);
               })
               console.log("successfully uploaded");
               this.postText = false;
             this.$emit('close');
           },

              
           onFileChange(e) {
      const file = e.target.files[0];
      console.log(file);
      this.filename =file.name;
      this.filePath = file; 
      
      this.url = URL.createObjectURL(file);
    },
     addPost(e) {
        console.log("clicked");
        
        
       const result = Storage.put(this.filename, this.filePath, {
          
           contentType:this.filePath.type,
    progressCallback(progress) {
        if(`${progress.loaded}` === `${progress.total}` ){
          
         
      
        }
        console.log(`Uploaded: ${progress.loaded}/${progress.total}`);
  },
}).then((result) =>{

    this.addPostToDb();
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
  background-color: white;
  
  height: 100vh;
flex-basis: 30%;
  
 
}
</style>