<template>
  <div>
      
    <amplify-authenticator v-if="authState !== 'signedin'" class="signIn" />
    <div v-if="authState === 'signedin' && user">
       
       <update-profile :email="user.attributes.email" :username="user.username"></update-profile>
     <button v-on:click="signOut">Sign Out</button>
    </div>
  </div>
</template>

<script>
import { onAuthUIStateChange } from '@aws-amplify/ui-components'
import { Auth }  from 'aws-amplify';
import UpdateProfile from '../components/UpdateProfile.vue';

export default {
  components: { UpdateProfile },
  created() {
    onAuthUIStateChange((authState, authData) => {
      this.authState = authState;
      this.user = authData;
      console.log(this.user);
    })
  },
  data() {
    return { user: undefined, authState: undefined }
  },
  methods: {
    signOut: async () => Auth.signOut()
  },
  beforeDestroy() {
    return onAuthUIStateChange;
  }
}
</script>

<style>


:root {
  --amplify-primary-color: #4287f5;
  --amplify-primary-tint: #005cf0;
  --amplify-primary-shade: #005cf0;
}
</style>