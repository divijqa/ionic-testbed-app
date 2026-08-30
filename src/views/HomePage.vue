<template>
  <ion-page>
    <!-- App Bar Header Layout -->
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
      <!-- Hidden Header for iOS Collapsing Toolbar Effect -->
      <ion-header collapse="condense">
        <ion-toolbar>
          <ion-title size="large">BizOps Portal</ion-title>
        </ion-toolbar>
      </ion-header>

      <!-- Section 1: Quick Business Metric Cards -->
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

      <!-- Section 2: Interactive Operational Grid -->
      <h3 class="section-title">Core Modules</h3>
      
      <ion-list lines="none" data-testid="modules-action-list">
        <!-- Module Link 1 -->
        <ion-item class="custom-list-item" button router-link="/data-list" data-testid="module-item-inventory">
          <ion-icon slot="start" :icon="cubeOutline" color="primary"></ion-icon>
          <ion-label>
            <h2>Inventory Management</h2>
            <p>Track warehouse stock and order dispatches</p>
          </ion-label>
          <ion-icon slot="end" :icon="chevronForwardOutline" color="medium"></ion-icon>
        </ion-item>

        <!-- Module Link 2 (Placeholder) -->
        <ion-item class="custom-list-item" button data-testid="module-item-clients">
          <ion-icon slot="start" :icon="peopleOutline" color="success"></ion-icon>
          <ion-label>
            <h2>Client Accounts</h2>
            <p>Manage commercial contracts and CRM profiles</p>
          </ion-label>
          <ion-icon slot="end" :icon="chevronForwardOutline" color="medium"></ion-icon>
        </ion-item>
      </ion-list>

      <!-- Section 3: Platform Health Indicator -->
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
import { 
  personCircleOutline, cubeOutline, peopleOutline, chevronForwardOutline 
} from 'ionicons/icons';
</script>

<style scoped>
/* Clean layout spacing styles */
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
  color: var(--ion-color-dark);
}

.custom-list-item {
  --background: var(--ion-color-light);
  --border-radius: 12px;
  margin-bottom: 10px;
  padding: 4px 0;
}

.pulse-indicator {
  width: 8px;
  height: 8px;
  background-color: var(--ion-color-success);
  border-radius: 50%;
  margin-right: 8px;
  box-shadow: 0 0 0 0 rgba(45, 211, 111, 0.7);
  animation: pulse 1.8s infinite;
}

@keyframes pulse {
  0% { transform: scale(0.95); box-shadow: 0 0 0 0 rgba(45, 211, 111, 0.7); }
  70% { transform: scale(1); box-shadow: 0 0 0 6px rgba(45, 211, 111, 0); }
  100% { transform: scale(0.95); box-shadow: 0 0 0 0 rgba(45, 211, 111, 0); }
}
</style>
