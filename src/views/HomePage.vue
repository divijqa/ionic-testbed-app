<template>
  <ion-page>
    <ion-header :translucent="true">
      <ion-toolbar color="dark">
        <ion-title data-testid="dashboard-header-title">BizOps Portal</ion-title>
        <ion-buttons slot="end">
          <ion-button data-testid="profile-avatar-btn">
            <ion-icon slot="icon-only" :icon="personCircleOutline"></ion-icon>
          </ion-button>
        </ion-buttons>
      </ion-toolbar>
    </ion-header>

    <ion-content :fullscreen="true" class="ion-padding">
      <div class="metrics-grid">
        <ion-card color="primary" data-testid="metric-card-revenue">
          <ion-card-header>
            <ion-card-subtitle class="metric-label">Monthly Gross</ion-card-subtitle>
            <ion-card-title class="metric-value">$14,250</ion-card-title>
          </ion-card-header>
        </ion-card>

        <ion-card color="success" data-testid="metric-card-orders">
          <ion-card-header>
            <ion-card-subtitle class="metric-label">Active Dispatches</ion-card-subtitle>
            <ion-card-title class="metric-value">38 Active</ion-card-title>
          </ion-card-header>
        </ion-card>
      </div>

      <h3 class="section-title">Core Modules</h3>

      <ion-list lines="none" data-testid="modules-action-list">
        <ion-item class="custom-list-item" button router-link="/data-list" data-testid="module-item-inventory">
          <ion-icon slot="start" :icon="cubeOutline" color="primary"></ion-icon>
          <ion-label>
            <h2>Inventory Management</h2>
            <p>Track warehouse stock and order dispatches</p>
          </ion-label>
          <ion-icon slot="end" :icon="chevronForwardOutline" color="medium"></ion-icon>
        </ion-item>

        <ion-item class="custom-list-item" button router-link="/clients" data-testid="module-item-clients">
          <ion-icon slot="start" :icon="peopleOutline" color="success"></ion-icon>
          <ion-label>
            <h2>Client Accounts</h2>
            <p>Manage commercial contracts and CRM profiles</p>
          </ion-label>
          <ion-icon slot="end" :icon="chevronForwardOutline" color="medium"></ion-icon>
        </ion-item>
      </ion-list>

      <div class="ion-text-center ion-padding-top">
        <ion-chip color="medium" data-testid="system-health-status">
          <div class="pulse-indicator"></div>
          <ion-label>System Status: Core Engine Online</ion-label>
        </ion-chip>
      </div>
    </ion-content>
  </ion-page>
</template>

<script setup>
import {
  IonContent, IonHeader, IonPage, IonTitle, IonToolbar,
  IonCard, IonCardHeader, IonCardSubtitle, IonCardTitle,
  IonList, IonItem, IonLabel, IonIcon, IonButtons, IonButton, IonChip
} from '@ionic/vue';
import { personCircleOutline, cubeOutline, peopleOutline, chevronForwardOutline } from 'ionicons/icons';
</script>

<style scoped>
.metrics-grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 8px;
  margin-bottom: 16px;
}

.metric-label {
  font-size: 11px;
  text-transform: uppercase;
  letter-spacing: 0.5px;
  opacity: 0.8;
}

.metric-value {
  font-size: 20px;
  font-weight: 700;
  margin-top: 4px;
}

.section-title {
  font-size: 16px;
  font-weight: 600;
  padding-left: 8px;
  margin-top: 24px;
  margin-bottom: 12px;
}

.custom-list-item {
  --background: var(--ion-color-light);
  --border-radius: 12px;
  margin-bottom: 10px;
}

.pulse-indicator {
  width: 8px;
  height: 8px;
  background-color: var(--ion-color-success);
  border-radius: 50%;
  margin-right: 8px;
}
</style>
