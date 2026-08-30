<template>
  <ion-page :class="{ 'dark-theme-override': isDarkMode }">
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
      <ion-searchbar v-model="searchQuery" placeholder="Search stock code..." data-testid="inventory-search-input" class="custom-searchbar"></ion-searchbar>

      <div v-if="filteredItems.length === 0" class="empty-state ion-text-center ion-padding" data-testid="empty-search-state">
        <ion-icon :icon="searchOutline" size="large" color="medium"></ion-icon>
        <h4>No Matching Assets Found</h4>
      </div>

      <ion-list v-else lines="none" class="transparent-list" data-testid="inventory-asset-list">
        <ion-card v-for="item in filteredItems" :key="item.id" :data-testid="'asset-card-' + item.sku" class="asset-card">
          <ion-card-header>
            <div class="card-header-row">
              <ion-badge :color="getBadgeColor(item.status)" data-testid="asset-status-badge">{{ item.status }}</ion-badge>
              <span class="sku-text">{{ item.sku }}</span>
            </div>
            <ion-card-title class="asset-title">{{ item.name }}</ion-card-title>
          </ion-card-header>
          <ion-card-content>
            <div class="meta-row">
              <span>Location: <strong>{{ item.location }}</strong></span>
              <span>Qty: <strong :class="{ 'low-stock': item.quantity < 20 }">{{ item.quantity }}</strong></span>
            </div>
          </ion-card-content>
        </ion-card>
      </ion-list>

      <ion-fab slot="fixed" vertical="bottom" horizontal="end">
        <ion-fab-button @click="openModal" color="success" data-testid="add-asset-fab">
          <ion-icon :icon="addOutline"></ion-icon>
        </ion-fab-button>
      </ion-fab>

      <ion-modal :is-open="isModalOpen" @didDismiss="closeModal" data-testid="add-asset-modal">
        <ion-header>
          <ion-toolbar color="dark">
            <ion-title>Create New Asset Record</ion-title>
            <ion-buttons slot="end">
              <ion-button @click="closeModal" data-testid="modal-close-btn">Cancel</ion-button>
            </ion-buttons>
          </ion-toolbar>
        </ion-header>
        <ion-content class="ion-padding">
          <ion-item class="form-item">
            <ion-input v-model="newAsset.name" label="Asset Name" label-placement="floating" placeholder="e.g. Server Racks" data-testid="input-asset-name"></ion-input>
          </ion-item>
          <ion-item class="form-item">
            <ion-input v-model="newAsset.sku" label="SKU Code" label-placement="floating" placeholder="e.g. SRV-77-X" data-testid="input-asset-sku"></ion-input>
          </ion-item>
          <ion-item class="form-item">
            <ion-input v-model="newAsset.location" label="Warehouse Location" label-placement="floating" placeholder="e.g. Row C2" data-testid="input-asset-location"></ion-input>
          </ion-item>
          <ion-item class="form-item">
            <ion-input v-model.number="newAsset.quantity" type="number" label="Initial Units" label-placement="floating" placeholder="0" data-testid="input-asset-quantity"></ion-input>
          </ion-item>
          <ion-button expand="block" color="success" class="ion-margin-top" @click="saveAsset" data-testid="modal-submit-btn">
            Publish to Master Ledger
          </ion-button>
        </ion-content>
      </ion-modal>
    </ion-content>
  </ion-page>
</template>

<script setup>
import { ref, computed } from 'vue';
import {
  IonContent, IonHeader, IonPage, IonTitle, IonToolbar, IonButtons, IonBackButton,
  IonSearchbar, IonList, IonCard, IonCardHeader, IonCardTitle, IonCardContent, IonBadge,
  IonButton, IonIcon, IonFab, IonFabButton, IonModal, IonItem, IonInput
} from '@ionic/vue';
import { moonOutline, sunnyOutline, searchOutline, addOutline } from 'ionicons/icons';

const isDarkMode = ref(false);
const searchQuery = ref('');
const isModalOpen = ref(false);

const stockItems = ref([
  { id: 1, sku: 'TSLA-992-X', name: 'Lithium Battery Packs (Grade A)', location: 'Warehouse Row B4', quantity: 142, status: 'In Stock' },
  { id: 2, sku: 'APPL-104-M', name: 'Silicon Core Processors (M-Series)', location: 'Cleanroom Drawer 2', quantity: 85, status: 'In Stock' }
]);

const newAsset = ref({ name: '', sku: '', location: '', quantity: '' });

const filteredItems = computed(() => {
  if (!searchQuery.value.trim()) return stockItems.value;
  return stockItems.value.filter(item => item.name.toLowerCase().includes(searchQuery.value.toLowerCase()));
});

const openModal = () => isModalOpen.value = true;
const closeModal = () => isModalOpen.value = false;

const saveAsset = () => {
  if (!newAsset.value.name || !newAsset.value.sku) return;
  stockItems.value.push({
    id: Date.now(),
    sku: newAsset.value.sku.toUpperCase(),
    name: newAsset.value.name,
    location: newAsset.value.location || 'Unassigned Dock',
    quantity: Number(newAsset.value.quantity) || 0,
    status: Number(newAsset.value.quantity) > 20 ? 'In Stock' : 'Low Stock'
  });
  newAsset.value = { name: '', sku: '', location: '', quantity: '' };
  closeModal();
};

const toggleTheme = () => isDarkMode.value = !isDarkMode.value;
const getBadgeColor = (status) => status === 'In Stock' ? 'success' : 'warning';
</script>

<style scoped>
.custom-searchbar { --border-radius: 12px; padding: 0 0 16px 0; }
.asset-card { margin: 0 0 12px 0; border-radius: 14px; background: var(--ion-card-background, #ffffff); }
.card-header-row { display: flex; justify-content: space-between; align-items: center; }
.sku-text { font-size: 11px; font-family: monospace; color: var(--ion-color-medium); }
.meta-row { display: flex; justify-content: space-between; font-size: 13px; }
.form-item { margin-bottom: 12px; --background: var(--ion-color-light); border-radius: 8px; }
.dark-theme-override { --ion-background-color: #121212; --ion-text-color: #ffffff; --ion-card-background: #1e1e1e; }
</style>
