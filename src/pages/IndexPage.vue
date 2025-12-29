<template>
  <q-page class="q-pa-md bg-grey-2">
    <!-- Header Section -->
    <div class="row items-center justify-between q-mb-md">
      <div>
        <div class="text-h5 text-weight-bold text-grey-9">Dashboard Overview</div>
        <div class="text-caption text-grey-6">Welcome back, Admin</div>
      </div>
      <q-btn unelevated color="primary" label="New Report" icon="add" />
    </div>

    <!-- Stats Row -->
    <div class="row q-col-gutter-md q-mb-lg">
      <div class="col-12 col-sm-6 col-md-3">
        <q-card class="my-card bg-gradient-blue text-white shadow-2">
          <q-card-section>
            <div class="text-caption text-blue-1">Total Customers</div>
            <div class="text-h4 text-weight-bolder q-mt-sm">1,240</div>
            <div class="row items-center q-mt-sm">
              <q-icon name="trending_up" class="q-mr-xs" />
              <span>+15% from last month</span>
            </div>
          </q-card-section>
        </q-card>
      </div>

      <div class="col-12 col-sm-6 col-md-3">
        <q-card class="my-card bg-gradient-purple text-white shadow-2">
          <q-card-section>
            <div class="text-caption text-purple-1">Active Users</div>
            <div class="text-h4 text-weight-bolder q-mt-sm">850</div>
            <div class="row items-center q-mt-sm">
              <q-icon name="group" class="q-mr-xs" />
              <span>75% Engagement rate</span>
            </div>
          </q-card-section>
        </q-card>
      </div>

      <div class="col-12 col-sm-6 col-md-3">
        <q-card class="my-card bg-gradient-orange text-white shadow-2">
          <q-card-section>
            <div class="text-caption text-orange-1">New Orders</div>
            <div class="text-h4 text-weight-bolder q-mt-sm">45</div>
            <div class="row items-center q-mt-sm">
              <q-icon name="shopping_cart" class="q-mr-xs" />
              <span>Today's stats</span>
            </div>
          </q-card-section>
        </q-card>
      </div>

      <div class="col-12 col-sm-6 col-md-3">
        <q-card class="my-card bg-white text-grey-9 shadow-2 border-l-primary">
          <q-card-section>
            <div class="text-caption text-grey-6">Pending Tasks</div>
            <div class="text-h4 text-weight-bolder q-mt-sm">5</div>
            <div class="row items-center q-mt-sm text-grey-6">
              <q-icon name="assignment" class="q-mr-xs" />
              <span>Action required</span>
            </div>
          </q-card-section>
        </q-card>
      </div>
    </div>

    <div class="row q-col-gutter-md">
      <!-- Chart Section -->
      <div class="col-12 col-md-8">
        <q-card class="shadow-2" style="border-radius: 12px">
          <q-card-section class="row items-center justify-between">
            <div class="text-subtitle1 text-weight-bold">Revenue Growth</div>
            <q-btn-toggle
              v-model="chartPeriod"
              toggle-color="primary"
              flat
              dense
              :options="[
                { label: 'Daily', value: 'd' },
                { label: 'Weekly', value: 'w' },
                { label: 'Monthly', value: 'm' },
              ]"
            />
          </q-card-section>
          <q-separator inset />
          <q-card-section class="q-pa-md">
            <div style="height: 350px; position: relative">
              <Line :data="chartData" :options="chartOptions" />
            </div>
          </q-card-section>
        </q-card>
      </div>

      <!-- Recent Activity -->
      <div class="col-12 col-md-4">
        <q-card class="shadow-2 full-height" style="border-radius: 12px">
          <q-card-section>
            <div class="text-subtitle1 text-weight-bold">Recent Updates</div>
          </q-card-section>

          <q-list class="q-px-sm">
            <q-item v-for="n in 5" :key="n" class="q-py-md">
              <q-item-section avatar>
                <q-avatar size="md" color="blue-1" text-color="blue" icon="widgets" rounded />
              </q-item-section>
              <q-item-section>
                <q-item-label class="text-weight-medium">System Update #{{ n }}</q-item-label>
                <q-item-label caption class="text-grey">Modules updated successfully</q-item-label>
              </q-item-section>
              <q-item-section side>
                <span class="text-caption text-grey">{{ n }}h ago</span>
              </q-item-section>
            </q-item>
          </q-list>
          <q-card-actions align="center">
            <q-btn flat label="View All History" color="primary" no-caps />
          </q-card-actions>
        </q-card>
      </div>
    </div>
  </q-page>
</template>

<script setup>
import { ref } from 'vue'
import {
  Chart as ChartJS,
  CategoryScale,
  LinearScale,
  PointElement,
  LineElement,
  Title,
  Tooltip,
  Legend,
  Filler,
} from 'chart.js'
import { Line } from 'vue-chartjs'

ChartJS.register(
  CategoryScale,
  LinearScale,
  PointElement,
  LineElement,
  Title,
  Tooltip,
  Legend,
  Filler,
)

const chartPeriod = ref('w')

const chartData = {
  labels: ['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun'],
  datasets: [
    {
      label: 'Growth',
      backgroundColor: (ctx) => {
        const canvas = ctx.chart.ctx
        const gradient = canvas.createLinearGradient(0, 0, 0, 400)
        gradient.addColorStop(0, 'rgba(54, 153, 255, 0.5)')
        gradient.addColorStop(1, 'rgba(54, 153, 255, 0.0)')
        return gradient
      },
      borderColor: '#3699FF',
      borderWidth: 3,
      pointBackgroundColor: '#fff',
      pointBorderColor: '#3699FF',
      pointBorderWidth: 2,
      data: [45, 59, 32, 50, 45, 78, 60],
      tension: 0.4,
      fill: true,
    },
  ],
}

const chartOptions = {
  responsive: true,
  maintainAspectRatio: false,
  plugins: {
    legend: {
      display: false,
    },
    tooltip: {
      backgroundColor: '#1e1e2d',
      titleColor: '#fff',
      bodyColor: '#fff',
      padding: 10,
      cornerRadius: 8,
      displayColors: false,
    },
  },
  scales: {
    y: {
      grid: {
        display: true,
        color: '#f3f6f9',
        borderDash: [5, 5],
      },
      ticks: {
        color: '#b5b5c3',
      },
    },
    x: {
      grid: {
        display: false,
      },
      ticks: {
        color: '#b5b5c3',
      },
    },
  },
}
</script>

<style scoped>
.my-card {
  border-radius: 12px;
  transition: transform 0.3s;
}
.my-card:hover {
  transform: translateY(-5px);
}

.bg-gradient-blue {
  background: linear-gradient(135deg, #3699ff 0%, #2f80ed 100%);
}
.bg-gradient-purple {
  background: linear-gradient(135deg, #8e2de2 0%, #4a00e0 100%);
}
.bg-gradient-orange {
  background: linear-gradient(135deg, #f2994a 0%, #f2c94c 100%);
}

.border-l-primary {
  border-left: 4px solid #3699ff;
}
</style>
