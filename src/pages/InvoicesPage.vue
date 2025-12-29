<template>
  <q-page class="q-pa-md bg-grey-2">
    <q-card class="shadow-4 no-border-radius" style="border-radius: 12px; overflow: hidden">
      <q-card-section class="row items-center justify-between q-pa-lg bg-white">
        <div class="row items-center">
          <q-icon name="receipt" size="md" color="primary" class="q-mr-sm" />
          <div class="text-h6 text-grey-9">Invoice Management</div>
        </div>

        <div class="row q-gutter-sm">
          <q-btn outline color="primary" label="Export" icon="ios_share" class="q-px-md" />
          <q-btn
            unelevated
            color="primary"
            label="New Invoice"
            icon="add"
            class="q-px-md"
            @click="showAddDialog = true"
          />
        </div>
      </q-card-section>

      <q-separator />

      <q-table
        :rows="rows"
        :columns="columns"
        row-key="id"
        :filter="filter"
        :pagination="initialPagination"
        flat
        class="custom-table"
      >
        <template v-slot:top-right>
          <q-input
            outlined
            dense
            debounce="300"
            v-model="filter"
            placeholder="Search Invoices"
            class="q-mr-sm"
          >
            <template v-slot:append>
              <q-icon name="search" />
            </template>
          </q-input>
        </template>

        <template v-slot:header="props">
          <q-tr :props="props">
            <q-th
              v-for="col in props.cols"
              :key="col.name"
              :props="props"
              class="text-weight-bold text-uppercase text-grey-6 bg-grey-1"
            >
              {{ col.label }}
            </q-th>
          </q-tr>
        </template>

        <template v-slot:body-cell-status="props">
          <q-td :props="props">
            <q-badge
              :color="getStatusColor(props.row.status)"
              rounded
              class="q-px-sm q-py-xs text-weight-bold"
              :label="props.row.status"
            />
          </q-td>
        </template>

        <template v-slot:body-cell-amount="props">
          <q-td :props="props" class="text-weight-bold text-grey-9"> ${{ props.row.amount }} </q-td>
        </template>

        <template v-slot:body-cell-actions="props">
          <q-td :props="props">
            <q-btn flat round color="grey-7" icon="visibility" size="sm" />
            <q-btn flat round color="primary" icon="edit" size="sm" />
            <q-btn flat round color="red" icon="delete_outline" size="sm" />
          </q-td>
        </template>
      </q-table>
    </q-card>

    <!-- Add Invoice Dialog -->
    <q-dialog v-model="showAddDialog">
      <q-card style="min-width: 500px">
        <q-card-section class="row items-center q-pb-none">
          <div class="text-h6">Create New Invoice</div>
          <q-space />
          <q-btn icon="close" flat round dense v-close-popup />
        </q-card-section>

        <q-card-section class="q-pt-md">
          <q-form class="q-gutter-md">
            <q-input outlined v-model="newInvoice.customer" label="Customer Name" />
            <div class="row q-col-gutter-sm">
              <div class="col-6">
                <q-input outlined v-model="newInvoice.date" type="date" label="Date" stack-label />
              </div>
              <div class="col-6">
                <q-input
                  outlined
                  v-model="newInvoice.dueDate"
                  type="date"
                  label="Due Date"
                  stack-label
                />
              </div>
            </div>
            <q-input outlined v-model="newInvoice.amount" label="Amount" prefix="$" type="number" />
            <q-select
              outlined
              v-model="newInvoice.status"
              :options="['Paid', 'Pending', 'Overdue']"
              label="Status"
            />

            <div align="right" class="q-mt-lg">
              <q-btn outline label="Cancel" color="primary" v-close-popup class="q-mr-sm" />
              <q-btn unelevated color="primary" label="Save Invoice" />
            </div>
          </q-form>
        </q-card-section>
      </q-card>
    </q-dialog>
  </q-page>
</template>

<script setup>
import { ref, reactive } from 'vue'

const showAddDialog = ref(false)
const filter = ref('')
const initialPagination = {
  sortBy: 'desc',
  descending: false,
  page: 1,
  rowsPerPage: 10,
}

const columns = [
  { name: 'id', align: 'left', label: '#ID', field: 'id', sortable: true },
  { name: 'customer', align: 'left', label: 'Customer', field: 'customer', sortable: true },
  { name: 'date', align: 'left', label: 'Date', field: 'date', sortable: true },
  { name: 'amount', align: 'right', label: 'Amount', field: 'amount', sortable: true },
  { name: 'status', align: 'center', label: 'Status', field: 'status', sortable: true },
  { name: 'actions', align: 'center', label: 'Actions', field: 'actions' },
]

const rows = ref([
  { id: 'INV-001', customer: 'Acme Corp', date: '2025-12-01', amount: '1,200.00', status: 'Paid' },
  {
    id: 'INV-002',
    customer: 'Global Tech',
    date: '2025-12-05',
    amount: '3,450.50',
    status: 'Pending',
  },
  {
    id: 'INV-003',
    customer: 'Local Services',
    date: '2025-12-10',
    amount: '500.00',
    status: 'Overdue',
  },
  {
    id: 'INV-004',
    customer: 'Design Studio',
    date: '2025-12-12',
    amount: '2,100.00',
    status: 'Paid',
  },
  {
    id: 'INV-005',
    customer: 'Marketing Guru',
    date: '2025-12-15',
    amount: '850.00',
    status: 'Pending',
  },
])

const newInvoice = reactive({
  customer: '',
  date: '',
  dueDate: '',
  amount: '',
  status: 'Pending',
})

function getStatusColor(status) {
  if (status === 'Paid') return 'green-5'
  if (status === 'Pending') return 'orange-5'
  if (status === 'Overdue') return 'red-5'
  return 'grey'
}
</script>

<style scoped>
.custom-table :deep(thead tr:first-child th) {
  /* bg-color is set in template */
}
</style>
