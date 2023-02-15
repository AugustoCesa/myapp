<template>
  <ion-page>
    <ion-header>
      <ion-toolbar>
        <ion-title>notifications</ion-title>
      </ion-toolbar>
     
     
    </ion-header>
    <ion-content :fullscreen="true">
      <div class="conteudo">
        <h1>Vamos colocar uma notificação aqui!</h1>

      </div>
<IonButton @click=""></IonButton>

    </ion-content>
  </ion-page>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import { IonPage, IonHeader, IonToolbar, IonTitle, IonContent, IonButton, } from '@ionic/vue';
import { PushNotifications } from '@capacitor/push-notifications';


export default defineComponent({
  name: 'Tab2Page',
  components: { IonHeader, IonToolbar, IonTitle, IonContent, IonPage, IonButton }
});

const addListeners = async () => {
  await PushNotifications.addListener('registration', token => {
    console.info('Registration token: ', token.value);
  });

  await PushNotifications.addListener('registrationError', err => {
    console.error('Registration error: ', err.error);
  });

  await PushNotifications.addListener('pushNotificationReceived', notification => {
    console.log('Push notification received: ', notification);
  });

  await PushNotifications.addListener('pushNotificationActionPerformed', notification => {
    console.log('Push notification action performed', notification.actionId, notification.inputValue);
  });
}

const registerNotifications = async () => {
  let permStatus = await PushNotifications.checkPermissions();

  if (permStatus.receive === 'prompt') {
    permStatus = await PushNotifications.requestPermissions();
  }

  if (permStatus.receive !== 'granted') {
    throw new Error('User denied permissions!');
  }

  await PushNotifications.register();
}

const getDeliveredNotifications = async () => {
  const notificationList = await PushNotifications.getDeliveredNotifications();
  console.log('delivered notifications', notificationList);
}






</script>





<style scoped="style">
  h1{
    color: rgb(255, 3, 146);
  }
  .conteudo{
    display: flex;
    align-items: center;
    justify-content: center;
  }
</style>