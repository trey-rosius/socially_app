<template>
    <div>
    <span>this is the home page</span>
    </div>
</template>

<script>
import { API } from 'aws-amplify';
import {getUser} from '../src/graphql/queries';

    export default {
        //Receiving parameters in Home component
mounted() {
    console.log('Params: ', this.$route.params);
    this.getUserDetails(this.$route.params.id);
   
       
},
methods:{
    async getUserDetails(userId){

         const user =  await API.graphql({
         query:getUser,
         variables:{id: userId}
       });

console.log(user.data.getUser);
    }
}
    }
</script>

<style lang="scss" scoped>

</style>