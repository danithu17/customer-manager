<template>
  <q-page class="q-pa-md">
    <q-card class="shadow-3">
      <q-card-section class="row items-center justify-between">
        <div class="text-h6">Customer List</div>
        <q-btn
          color="primary"
          icon="add"
          label="Add Customer"
          @click="showAddDialog = true"
          unelevated
        />
      </q-card-section>

      <q-separator />

      <q-table
        :rows="rows"
        :columns="columns"
        row-key="id"
        :filter="filter"
        :pagination="initialPagination"
        flat
      >
        <template v-slot:top-right>
          <q-input borderless dense debounce="300" v-model="filter" placeholder="Search">
            <template v-slot:append>
              <q-icon name="search" />
            </template>
          </q-input>
        </template>

        <template v-slot:body-cell-status="props">
          <q-td :props="props">
            <q-badge
              :color="props.row.status === 'Active' ? 'green' : 'grey-7'"
              rounded
              class="q-px-sm"
            >
              {{ props.row.status }}
            </q-badge>
          </q-td>
        </template>

        <template v-slot:body-cell-actions="props">
          <q-td :props="props">
            <q-btn flat round color="primary" icon="edit" size="sm" />
            <q-btn flat round color="red" icon="delete" size="sm" />
          </q-td>
        </template>
      </q-table>
    </q-card>

    <!-- Add Customer Dialog -->
    <q-dialog v-model="showAddDialog">
      <q-card style="min-width: 400px">
        <q-card-section>
          <div class="text-h6">Add New Customer</div>
        </q-card-section>

        <q-card-section class="q-pt-none">
          <q-form @submit="onSubmit" class="q-gutter-md">
            <q-input
              filled
              v-model="newCustomer.name"
              label="Full Name"
              :rules="[(val) => (val && val.length > 0) || 'Required']"
            />
            <q-input filled v-model="newCustomer.email" label="Email" type="email" />
            <q-input filled v-model="newCustomer.phone" label="Phone" />
            <q-select
              filled
              v-model="newCustomer.status"
              :options="['Active', 'Inactive']"
              label="Status"
            />

            <div align="right">
              <q-btn flat label="Cancel" v-close-popup />
              <q-btn color="primary" label="Save" type="submit" />
            </div>
          </q-form>
        </q-card-section>
      </q-card>
    </q-dialog>
  </q-page>
</template>

<script setup>
import { ref, reactive } from 'vue'
import { useQuasar } from 'quasar'

const $q = useQuasar()
const showAddDialog = ref(false)
const filter = ref('')
const initialPagination = {
  sortBy: 'desc',
  descending: false,
  page: 1,
  rowsPerPage: 10,
}

const columns = [
  {
    name: 'name',
    required: true,
    label: 'Name',
    align: 'left',
    field: (row) => row.name,
    sortable: true,
    style: 'font-weight: bold',
  },
  { name: 'email', align: 'left', label: 'Email', field: 'email', sortable: true },
  { name: 'phone', align: 'left', label: 'Phone', field: 'phone' },
  { name: 'status', align: 'center', label: 'Status', field: 'status', sortable: true },
  { name: 'actions', align: 'center', label: 'Actions', field: 'actions' },
]

const rows = ref([
  {
    id: 1,
    name: 'Kasun Perera',
    email: 'kasun@example.com',
    phone: '0712345678',
    status: 'Active',
  },
  {
    id: 2,
    name: 'Nimal Silva',
    email: 'nimal@example.com',
    phone: '0771231234',
    status: 'Inactive',
  },
  {
    id: 3,
    name: 'Amara Weerasekara',
    email: 'amara@example.com',
    phone: '0754567890',
    status: 'Active',
  },
  {
    id: 4,
    name: 'Kamal Gunaratne',
    email: 'kamal@example.com',
    phone: '0765432109',
    status: 'Active',
  },
  {
    id: 5,
    name: 'Sunil Shantha',
    email: 'sunil@example.com',
    phone: '0719876543',
    status: 'Inactive',
  },
  {
    id: 6,
    name: 'Chathurika De Silva',
    email: 'chathu@example.com',
    phone: '0722223333',
    status: 'Active',
  },
  {
    id: 7,
    name: 'Ruwan Kumara',
    email: 'ruwan@example.com',
    phone: '0788889999',
    status: 'Active',
  },
])

const newCustomer = reactive({
  name: '',
  email: '',
  phone: '',
  status: 'Active',
})

function onSubmit() {
  rows.value.push({
    id: rows.value.length + 1,
    name: newCustomer.name,
    email: newCustomer.email,
    phone: newCustomer.phone,
    status: newCustomer.status,
  })
  $q.notify({
    color: 'green-4',
    textColor: 'white',
    icon: 'check_circle',
    message: 'Customer Added Successfully',
  })
  showAddDialog.value = false
  // Reset form
  newCustomer.name = ''
  newCustomer.email = ''
  newCustomer.phone = ''
}
</script>
