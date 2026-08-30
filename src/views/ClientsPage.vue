<template>
  <ion-page>
    <ion-header>
      <ion-toolbar color="success">
        <ion-buttons slot="start">
          <ion-back-button default-href="/home" data-testid="back-to-dash-from-clients"></ion-back-button>
        </ion-buttons>
        <ion-title data-testid="clients-header-title">Commercial Accounts</ion-title>
      </ion-toolbar>
    </ion-header>

    <ion-content class="ion-padding">
      <ion-list lines="none" data-testid="clients-master-list">
        <ion-item-sliding v-for="client in clients" :key="client.id" :data-testid="'client-sliding-' + client.id">
          <ion-item class="client-item">
            <ion-avatar slot="start">
              <div class="avatar-placeholder">{{ client.name.charAt(0) }}</div>
            </ion-avatar>
            <ion-label>
              <h2 data-testid="client-name-label">{{ client.name }}</h2>
              <p>Contract Value: <strong>{{ client.value }}</strong></p>
              <ion-note :color="client.tier === 'Enterprise' ? 'primary' : 'medium'">Tier: {{ client.tier }}</ion-note>
            </ion-label>
          </ion-item>
        </ion-item-sliding>
      </ion-list>
    </ion-content>
  </ion-page>
</template>

<script setup>
import { ref } from 'vue';
import { IonPage, IonHeader, IonToolbar, IonTitle, IonContent, IonList, IonItem, IonLabel, IonAvatar, IonNote, IonButtons, IonBackButton, IonItemSliding } from '@ionic/vue';

const clients = ref([
  { id: 101, name: 'Acme Logistics Corp', value: '$84,000/yr', tier: 'Enterprise' },
  { id: 102, name: 'Global Vanguard Distro', value: '$32,500/yr', tier: 'Mid-Market' },
  { id: 103, name: 'Nexus Tech Systems', value: '$112,000/yr', tier: 'Enterprise' }
]);
</script>

<style scoped>
.client-item { --background: var(--ion-color-light); --border-radius: 12px; margin-bottom: 10px; }
.avatar-placeholder { width: 40px; height: 40px; background: var(--ion-color-success); color: white; display: flex; align-items: center; justify-content: center; font-weight: bold; border-radius: 50%; }
</style>
