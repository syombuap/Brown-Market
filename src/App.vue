<template>
  <ion-app>
    <ion-router-outlet />
    <ion-modal  ref="modal" @willDismiss="isLoginModalOpen=false" :is-open="isLoginModalOpen">
      <ion-header>
        <ion-toolbar>
          
          
          <ion-title>Login</ion-title>
          
        </ion-toolbar>
      </ion-header>
      <ion-content class="ion-padding">
        <ion-item>
          <ion-input
            label="Enter your name"
            label-placement="stacked"
            ref="input"
            type="text"
            placeholder="Your name"
          ></ion-input>
        </ion-item>
        <ion-button @click="signinRedirect()" expand="block">Block</ion-button>
        <ion-button @click="signinPopup()" expand="block">Block</ion-button>
      </ion-content>
    </ion-modal>
  </ion-app>

</template>

<script setup lang="ts">
import { IonButtons, IonButton, IonModal, IonHeader, IonToolbar, IonContent, IonTitle } from '@ionic/vue';
import { IonApp, IonRouterOutlet } from '@ionic/vue';
import { ref, watch } from 'vue';
import { useCurrentUser, useFirebaseAuth } from 'vuefire';
import {
  getRedirectResult,
  signInWithPopup,
  signInWithRedirect,
  signOut,
} from 'firebase/auth'
const isLoginModalOpen = ref(false);
const auth:any = useFirebaseAuth()
// App.vue
const user = useCurrentUser()

import { GoogleAuthProvider } from 'firebase/auth'
 const googleAuthProvider = new GoogleAuthProvider()



watch(user, async (currentUser, previousUser) => {
  // redirect to login if they logout and the current
  // route is only for authenticated users
  console.log(currentUser, )

  if (
    !currentUser 
  ) {
    console.log(isLoginModalOpen.value)
    return isLoginModalOpen.value=true

  }

  // redirect the user if they are logged in but were
  // rejected because the user wasn't ready yet, logged in
  // then got back to this page
  if (currentUser ) {
    console.log(isLoginModalOpen.value)
    return isLoginModalOpen.value =false
  }
})
// display errors if any
const error = ref(null)
function signinRedirect() {
  console.log('it is running')
  signInWithPopup(auth, googleAuthProvider).catch((reason) => {
    console.error('Failed signinRedirect', reason)
    error.value = reason
  })
}
function signinPopup() {
  error.value = null
  signInWithPopup(auth, googleAuthProvider).catch((reason) => {
    console.error('Failed sign', reason)
    error.value = reason
  })
}
</script>
