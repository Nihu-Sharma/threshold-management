<template>
  <ion-page>
    <ion-header :translucent="true">
      <ion-toolbar>
        <ion-menu-button slot="start" />
        <ion-title>{{ $t("Settings") }}</ion-title>
      </ion-toolbar>
    </ion-header>
    
    <ion-content>
       <!-- Select eCom store -->
      <ion-item>
        <ion-icon :icon="globeOutline" slot="start" />
        <ion-label>{{$t("Shop")}}</ion-label>
        <ion-select interface="popover" :value="currentEComStore.productStoreId" @ionChange="setEComStore($event)">
          <ion-select-option v-for="store in (userProfile ? userProfile.stores : [])" :key="store.productStoreId" :value="store.productStoreId" >{{ store.storeName }}</ion-select-option>
        </ion-select>
      </ion-item>

      <!-- OMS information -->
      <ion-item>
        <ion-icon :icon="codeWorkingOutline" slot="start"/>
        <ion-label>{{ $t("OMS") }}</ion-label>
        <p slot="end">{{ baseURL ? baseURL : instanceUrl }}</p>
      </ion-item>

      <!-- Profile of user logged in -->
      <ion-item>
        <ion-icon :icon="personCircleOutline" slot="start" />
        <ion-label>{{ userProfile !== null ? userProfile.partyName : '' }}</ion-label>
        <ion-button slot="end" fill="outline" color="dark" @click="logout()">{{ $t("Logout") }}</ion-button>
      </ion-item>

    </ion-content>
  </ion-page>
</template>

<script lang="ts">
import { IonBackButton, IonButton, IonContent, IonHeader,IonIcon, IonItem, IonLabel, IonMenuButton, IonPage, IonSelect, IonSelectOption, IonTitle, IonToolbar, popoverController } from '@ionic/vue';
import { defineComponent } from 'vue';
import { codeWorkingOutline, ellipsisVertical, globeOutline, personCircleOutline, storefrontOutline} from 'ionicons/icons'
import { mapGetters, useStore } from 'vuex';
import { useRouter } from 'vue-router';

export default defineComponent({
  name: 'Settings',
  components: {
    IonButton, 
    IonContent, 
    IonHeader, 
    IonIcon,
    IonItem, 
    IonLabel, 
    IonMenuButton,
    IonPage, 
    IonSelect, 
    IonSelectOption,
    IonTitle, 
    IonToolbar
  },
  data() {
    return {
      baseURL: process.env.VUE_APP_BASE_URL
    };
  },
  computed: {
    ...mapGetters({
      userProfile: 'user/getUserProfile',
      currentEComStore: 'user/getCurrentEComStore',
      instanceUrl: 'user/getInstanceUrl'
    })
  },
  methods: {
    setEComStore(store: any) {
      if(this.userProfile) {
        this.store.dispatch('user/setEcomStore', {
          'eComStore': this.userProfile.stores.find((str: any) => str.productStoreId == store['detail'].value)
        })
      }
    },
    logout () {
      this.store.dispatch('user/logout').then(() => {
        this.router.push('/login');
      })
    }
  },
  setup(){
    const store = useStore();
    const router = useRouter();

    return {
      codeWorkingOutline,
      ellipsisVertical,
      globeOutline,
      personCircleOutline,
      storefrontOutline,
      store,
      router
    }
  }
});
</script>
