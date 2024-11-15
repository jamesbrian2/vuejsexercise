<script setup>
import { ref } from 'vue';

let nameList = ref([]);
let name = ref(null);
let address = ref(null);
let age = ref(null);
let contactNumber = ref(null);

let selectedName = ref(null);

const addName = () => {
  if (validateName(name.value) && validateAddress(address.value)) {
    let id = nameList.value.length + 1;
    nameList.value.push({
      id,
      name: name.value,
      address: address.value,
      age: age.value,
      contactNumber: contactNumber.value,
      is_done: false
    });
    name.value = null;
    address.value = null;
    age.value = null;
    contactNumber.value = null;
  }
};

const validateName = (new_name) => {
  const index = nameList.value.findIndex((n) => n.name === new_name && n.id !== selectedName.value?.id);
  if (index !== -1) {
    alert("Name already exists!");
    return false;
  }
  return true;
};

const validateAddress = (new_address) => {
  const index = nameList.value.findIndex((a) => a.address === new_address && a.id !== selectedName.value?.id);
  if (index !== -1) {
    alert("Address already exists!");
    return false;
  }
  return true;
};

const selectName = (row) => {
  selectedName.value = row;
  name.value = row.name;
  address.value = row.address;
  age.value = row.age;
  contactNumber.value = row.contactNumber;
};

const confirmEdit = () => {
  if (window.confirm('Are you sure you want to update this entry?')) {
    updateName();
  }
};

const updateName = () => {
  if (validateName(name.value) && validateAddress(address.value)) {
    const index = nameList.value.findIndex((n) => n.id === selectedName.value.id);
    if (index !== -1) {
      nameList.value[index].name = name.value;
      nameList.value[index].address = address.value;
      nameList.value[index].age = age.value;
      nameList.value[index].contactNumber = contactNumber.value;
    }
    name.value = null;
    address.value = null;
    age.value = null;
    contactNumber.value = null;
    selectedName.value = null;
  }
};

const removeName = (row) => {
  if (confirm(`Are you sure you want to delete ${row.name} from ${row.address}?`)) {
    const index = nameList.value.findIndex((n) => n.id === row.id);
    if (index !== -1) {
      nameList.value.splice(index, 1);
    }
  }
};

const handleSubmit = () => {
  if (!selectedName.value) {
    addName();
  } else {
    confirmEdit();
  }
};

const handleEdit = () => {
  if (window.confirm('Are you sure you want to update this entry?')) {
    updateName();
  }
};
</script>

<template>
  <div class="container mt-5">
  <div class="row">
    <div class="col-md-4">
      <div class="card shadow-sm">
        <div class="card-header">
          <h5 class="mb-0">Add / Edit User</h5>
        </div>
        <div class="card-body">
          <form @submit.prevent="handleSubmit">
            <div class="mb-3">
              <label for="name" class="form-label">Name</label>
              <div class="d-flex">
                <input v-model="name" type="text" class="form-control" id="name" placeholder="Enter name">
                <input v-model="address" type="text" class="form-control ms-2" id="address" placeholder="Enter address">
              </div>
            </div>
            <div class="mb-3">
              <label for="age" class="form-label">Age</label>
              <input v-model="age" type="number" class="form-control" id="age" placeholder="Enter age" min="1">
            </div>
            <div class="mb-3">
              <label for="contactNumber" class="form-label">Contact Number</label>
              <input v-model="contactNumber" type="text" class="form-control" id="contactNumber" placeholder="Enter contact number">
            </div>
            <div class="d-flex justify-content-end">
              <button class="btn btn-primary" type="submit">
                <i class="bi bi-plus-circle-fill"></i> Add
              </button>
              <button v-if="selectedName" class="btn btn-warning ms-2" type="button" @click="handleEdit">
                <i class="bi bi-pencil-fill"></i> Edit
              </button>
            </div>
          </form>
        </div>
      </div>
    </div>
  


      <!-- Bottom container for displaying the list -->
      <div class="col-md-12 mt-4">
        <div class="card shadow-sm">
          <div class="card-header">
            <h5 class="mb-0">User List</h5>
          </div>
          <div class="card-body">
            <div v-for="row in nameList" :key="row.id" class="toast show mt-3 w-100">
              <div class="toast-header">
                <strong class="me-auto">{{ row.name }}, {{ row.address }} (Age: {{ row.age }}, Contact: {{ row.contactNumber }})</strong>
                <div class="d-flex">
                  <button class="btn btn-warning btn-sm ms-1" @click.stop="selectName(row)">
                    <i class="bi bi-pencil-fill"></i> Edit
                  </button>
                  <button class="btn btn-danger btn-sm ms-1" @click.stop="removeName(row)">
                    <i class="bi bi-trash-fill"></i> Delete
                  </button>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.container {
  max-width: 1200px;
}

.card {
  margin-bottom: 20px;
}

button {
  margin-left: 5px;
}

.mt-4 {
  margin-top: 20px;
}


</style>
