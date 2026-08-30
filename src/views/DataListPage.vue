<template>
  <ion-page :class="{ 'dark-theme-override': isDarkMode }">
    <!-- Header Layout with Back Navigation and Theme Toggle -->
    <ion-header :translucent="true">
      <ion-toolbar :color="isDarkMode ? 'dark' : 'primary'">
        <ion-buttons slot="start">
          <ion-back-button default-href="/home" data-testid="back-to-dashboard-btn"></ion-back-button>
        </ion-buttons>
        <ion-title data-testid="inventory-header-title">Inventory Master List</ion-title>
        <ion-buttons slot="end">
          <ion-button @click="toggleTheme" data-testid="theme-toggle-btn">
            <ion-icon slot="icon-only" :icon="isDarkMode ? sunnyOutline : moonOutline"></ion-icon>
          </ion-button>
        </ion-buttons>
      </ion-toolbar>
    </ion-header>

    <ion-content :fullscreen="true" class="ion-padding">
      <!-- Live Interactive Search Bar -->
      <ion-searchbar 
        v-model="searchQuery" 
        placeholder="Search stock code or location..." 
        data-testid="inventory-search-input"
        class="custom-searchbar">
      </ion-searchbar>

      <!-- Empty State View Condition -->
      <div v-if="filteredItems.length === 0" class="empty-state ion-text-center ion-padding" data-testid="empty-search-state">
        <ion-icon :icon="searchOutline" size="large" color="medium"></ion-icon>
        <h4>No Matching Assets Found</h4>
        <p>Verify your locator strategy or target parameters.</p>
      </div>

      <!-- Reactive Asset List Layout -->
      <ion-list v-else lines="none" class="transparent-list" data-testid="inventory-asset-list">
        <ion-card 
          v-for="item in filteredItems" 
          :key="item.id" 
          :data-testid="'asset-card-' + item.sku"
          class="asset-card">
          <ion-card-header>
            <div class="card-header-row">
              <ion-badge :color="getBadgeColor(item.status)" data-testid="asset-status-badge">
                {{ item.status }}
              </ion-badge>
              <span class="sku-text">{{ item.sku }}</span>
            </div>
            <ion-card-title class="asset-title">{{ item.name }}</ion-card-title>
          </ion-card-header>
          
          <ion-card-content>
            <div class="meta-row">
              <span>📍 Location: <strong>{{ item.location }}</strong></span>
              <span>📦 Qty: <strong :class="{ 'low-stock': item.quantity < 20 }">{{ item.quantity }}</strong></span>
            </div>
          </ion-card-content>
        </ion-card>
      </ion-list>
    </ion-content>
  </ion-page>
</template>

<script setup>
import { ref, computed } from 'vue';
import { 
  IonContent, IonHeader, IonPage, IonTitle, IonToolbar, IonButtons, IonBackButton,
  IonSearchbar, IonList, IonCard, IonCardHeader, IonCardTitle, IonCardContent, IonBadge, IonButton, IonIcon
} from '@ionic/vue';
import { moonOutline, sunnyOutline, searchOutline } from 'ionicons/icons';

// Mock Platform Database State
const stockItems = ref([
  { id: 1, sku: 'TSLA-992-X', name: 'Lithium Battery Packs (Grade A)', location: 'Warehouse Row B4', quantity: 142, status: 'In Stock' },
  { id: 2, sku: 'APPL-104-M', name: 'Silicon Core Processors (M-Series)', location: 'Cleanroom Drawer 2', quantity: 85, status: 'In Stock' },
  { id: 3, sku: 'NVDA-551-G', name: 'AI Tensor Tensor Matrix Units', location: 'Inbound Dock A', quantity: 12, status: 'Low Stock' },
  { id: 4, sku: 'AMZN-004-K', name: 'Robotic Drive Unit Spare Gears', location: 'Warehouse Row F9', quantity: 0, status: 'Out of Stock' }
]);

const searchQuery = ref('');
const isDarkMode = ref(false);

// Reactive Filter Implementation
const filteredItems = computed(() => {
  if (!searchQuery.value.trim()) return stockItems.value;
  return stockItems.value.filter(item => 
    item.name.toLowerCase().includes(searchQuery.value.toLowerCase()) ||
    item.sku.toLowerCase().includes(searchQuery.value.toLowerCase()) ||
    item.location.toLowerCase().includes(searchQuery.value.toLowerCase())
  );
});

// UI Event Actions
const toggleTheme = () => {
  isDarkMode.value = !isDarkMode.value;
};

const getBadgeColor = (status) => {
  if (status === 'In Stock') return 'success';
  if (status === 'Low Stock') return 'warning';
  return 'danger';
};
</script>

<style scoped>
.custom-searchbar {
  --border-radius: 12px;
  padding: 0 0 16px 0;
}

.asset-card {
  margin: 0 0 12px 0;
  border-radius: 14px;
  box-shadow: 0 4px 10px rgba(0,0,0,0.05);
  background: var(--ion-card-background, #ffffff);
}

.card-header-row {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 6px;
}

.sku-text {
  font-size: 11px;
  font-family: monospace;
  color: var(--ion-color-medium);
}

.asset-title {
  font-size: 16px;
  font-weight: 600;
}

.meta-row {
  display: flex;
  justify-content: space-between;
  font-size: 13px;
  color: var(--ion-color-step-600);
}

.low-stock {
  color: var(--ion-color-danger);
  font-weight: bold;
}

.empty-state {
  margin-top: 60px;
}

.empty-state h4 {
  font-weight: 600;
  margin: 12px 0 4px;
}

/* Localized Contextual Dark Mode Overrides */
.dark-theme-override {
  --ion-background-color: #121212;
  --ion-text-color: #ffffff;
  --ion-card-background: #1e1e1e;
}

.dark-theme-override .custom-searchbar {
  --background: #1e1e1e;
  --color: #ffffff;
}
</style>
