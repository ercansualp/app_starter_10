<template>
  <div class="space-y-8">
    <!-- Header -->
    <div>
      <h1 class="text-3xl font-bold text-text-primary">Gösterge Paneli</h1>
      <p class="text-text-secondary mt-1">İşletmenizin genel bir özetini burada bulabilirsiniz.</p>
    </div>

    <!-- Stat Cards -->
    <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-6">
      <div class="relative rounded-xl">
        <StatCard :icon="DollarSign" title="Toplam Gelir" :value="totalRevenue" />
        <LoadingOverlay :is-loading="statCardsLoading" :message="statCardsMessage" />
      </div>
      <div class="relative rounded-xl">
        <StatCard :icon="Users" title="Yeni Müşteriler" :value="newCustomers" />
        <LoadingOverlay :is-loading="statCardsLoading" :message="statCardsMessage" />
      </div>
      <div class="relative rounded-xl">
        <StatCard :icon="CreditCard" title="Aylık Satışlar" :value="monthlySales" />
        <LoadingOverlay :is-loading="statCardsLoading" :message="statCardsMessage" />
      </div>
      <div class="relative rounded-xl">
        <StatCard :icon="Activity" title="Aktif Projeler" :value="activeProjectsCount.toString()" />
        <LoadingOverlay :is-loading="statCardsLoading" :message="statCardsMessage" />
      </div>
    </div>

    <!-- Main Content Grid -->
    <div class="grid grid-cols-1 lg:grid-cols-3 gap-6">
      <!-- Dashboard Chart (Larger on left) -->
      <div class="lg:col-span-2 relative rounded-xl">
        <DashboardChart />
        <LoadingOverlay :is-loading="dashboardChartLoading" :message="dashboardChartMessage" />
      </div>
      
      <!-- Project Status (Smaller on right) -->
      <div class="lg:col-span-1 relative rounded-xl">
        <ProjectStatus />
        <LoadingOverlay :is-loading="projectStatusLoading" :message="projectStatusMessage" />
      </div>
    </div>
    
    <!-- Recent Sales Table -->
    <div class="relative rounded-xl">
      <RecentSales />
      <LoadingOverlay :is-loading="recentSalesLoading" :message="recentSalesMessage" />
    </div>

  </div>
</template>

<script setup>
import { ref, computed, onMounted } from 'vue'
import StatCard from '~/components/StatCard.vue'
import RecentSales from '~/components/RecentSales.vue'
import ProjectStatus from '~/components/ProjectStatus.vue'
import DashboardChart from '~/components/DashboardChart.vue'
import LoadingOverlay from '~/components/LoadingOverlay.vue' // Import the new component
import { useProjects } from '~/composables/useProjects'
import { DollarSign, Users, CreditCard, Activity } from 'lucide-vue-next'

const { projects } = useProjects()

// Loading states for each section
const statCardsLoading = ref(true)
const dashboardChartLoading = ref(true)
const projectStatusLoading = ref(true)
const recentSalesLoading = ref(true)

// Loading messages
const statCardsMessage = ref('Gösterge verileri yükleniyor...')
const dashboardChartMessage = ref('Gelir analizi yükleniyor...')
const projectStatusMessage = ref('Proje durumları yükleniyor...')
const recentSalesMessage = ref('Son satışlar yükleniyor...')

// Data for StatCards (simulated)
const totalRevenue = ref('₺0')
const newCustomers = ref('0')
const monthlySales = ref('₺0')

const activeProjectsCount = computed(() => {
  return projects.value.filter(p => p.isActive).length
})

// Simulate data loading
onMounted(() => {
  // Simulate Stat Cards loading
  setTimeout(() => {
    totalRevenue.value = '₺125,430'
    newCustomers.value = '+1,204'
    monthlySales.value = '+8,950'
    statCardsLoading.value = false
  }, 1500) // Simulate 1.5 seconds loading

  // Simulate Dashboard Chart loading
  setTimeout(() => {
    dashboardChartLoading.value = false
  }, 2000) // Simulate 2 seconds loading

  // Simulate Project Status loading
  setTimeout(() => {
    projectStatusLoading.value = false
  }, 1800) // Simulate 1.8 seconds loading

  // Simulate Recent Sales loading
  setTimeout(() => {
    recentSalesLoading.value = false
  }, 2200) // Simulate 2.2 seconds loading
})
</script>
