<script setup lang="ts">
// imports
import UITable from "./components/UITable.vue"

// data
const cols = [
  {
    name: "ID",
    key: "id"
  },
  {
    name: "Ismi",
    key: "name"
  },
  {
    name: "Familyasi",
    key: "surname"
  },
  {
    name: "Telefon raqami",
    key: "phone_number"
  },
  {
    name: "Pochta",
    key: "email"
  },
  {
    name: "Yashash manzili",
    key: "address"
  },
  {
    name: "Amallar",
    key: "actions"
  },
]

const users = [
  {
    id: 1,
    name: "Murod",
    email: "birnima@mail.com",
    phone_number: "+998 99 999 99 99",
    address: "Viloyat, Tuman",
    surname: "Baxromov"
  },
  {
    id: 2,
    name: "Murod 2",
    email: "birnima2@mail.com",
    phone_number: "+998 99 999 99 99",
    address: "Viloyat, Tuman",
    surname: "Baxromov 2"
  },
  {
    id: 3,
    name: "Murod 3",
    email: "birnima3@mail.com",
    phone_number: "+998 99 999 99 99",
    address: "Viloyat, Tuman",
    surname: "Baxromov 3"
  }
]

// methods
function sendUser(data: any) {
  alert("Ushbu olingan ma'lumotni siz console orqali ko'rishingiz mumkin!")
  console.log(data)
}

</script>

<template>
  <div class="p-4 bg-gray-100 h-screen">
    <span class="mt-4 text-lg">
      Foydalanuvchilar jadvali
    </span>
    <UITable :columns="cols" :rows="users" class="mt-4">
      <template #actions="props">
        <button class="border px-2 py-1 rounded-md cursor-pointer bg-orange-400 text-white" @click="sendUser(props)">
          edit
        </button>
      </template>
    </UITable>
  </div>
</template>