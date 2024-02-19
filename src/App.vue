<script setup lang="ts">
// imports
import { ref } from "vue";
import UITable from "./components/UITable.vue"
import UIDialog from "./components/UIDialog.vue"
import UIInput from "./components/UIInput.vue"

// types
interface IUserDataType {
  form: {
    dialog_model: boolean,
    dialog_title: string,
    editing: {
      data: any,
      value: boolean
    },
    deleting: {
      dialog_title: string,
      dialog_model: boolean,
      data: any
    }
    name: string,
    email: string,
    phone_number: string,
    address: string,
    surname: string
  }
}

// data
const udata = ref<IUserDataType>({
  form: {
    dialog_model: false,
    dialog_title: "",
    editing: {
      data: null,
      value: false
    },
    deleting: {
      data: null,
      dialog_model: false,
      dialog_title: "Foydalanuvchini o'shirish"
    },
    address: "",
    email: "",
    name: "",
    phone_number: "",
    surname: ""
  }
})
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

const users = ref<any[]>(JSON.parse(localStorage.getItem("users")) || [])

// methods
function showDialog(action_name: string, data?: any) {
  if (action_name === "create") {
    udata.value.form.editing.value = false
    udata.value.form.dialog_title = "Yangi foydalanuvchi yaratish!"
    udata.value.form.name = ""
    udata.value.form.surname = ""
    udata.value.form.email = ""
    udata.value.form.phone_number = ""
    udata.value.form.address = ""
    udata.value.form.dialog_model = true
  } else {
    udata.value.form.editing.value = true
    udata.value.form.editing.data = data
    udata.value.form.dialog_title = "Foydalanuvchini o'zgartirish!"
    udata.value.form.name = data.name
    udata.value.form.surname = data.surname
    udata.value.form.email = data.email
    udata.value.form.phone_number = data.phone_number
    udata.value.form.address = data.address
    udata.value.form.dialog_model = true
    console.log(data)
  }
}
function createNewUser() {
  if (!udata.value.form.name || !udata.value.form.surname || !udata.value.form.email || !udata.value.form.phone_number || !udata.value.form.address) {
    alert("Barcha bo'limni to'ldirishingiz shart!")
  } else {
    const user: { id?: number, name?: string, surname?: string, email?: string, phone_number?: string, address?: string } = {}
    user.id = Date.now()
    user.name = udata.value.form.name
    user.surname = udata.value.form.surname
    user.email = udata.value.form.email
    user.phone_number = udata.value.form.phone_number
    user.address = udata.value.form.address

    users.value.push(user)
    localStorage.setItem("users", JSON.stringify(users.value))
    udata.value.form.dialog_model = false
  }
}

function updateUserData() {
  if (!udata.value.form.name || !udata.value.form.surname || !udata.value.form.email || !udata.value.form.phone_number || !udata.value.form.address) {
    alert("Barcha bo'limni to'ldirishingiz shart!")
  } else {
    users.value = users.value.map((user) => {
      if (user.id === udata.value.form.editing.data.id) {
        user.name = udata.value.form.name
        user.surname = udata.value.form.surname
        user.email = udata.value.form.email
        user.phone_number = udata.value.form.phone_number
        user.address = udata.value.form.address
      }
      return user
    })
    localStorage.setItem("users", JSON.stringify(users.value))
    udata.value.form.dialog_model = false
  }
}
function showDeleteUserDialog(data: any) {
  udata.value.form.deleting.data = data
  udata.value.form.deleting.dialog_model = true
}

function deleteUser() {
  const new_users: any[] = []
  users.value.map((user) => {
    if(user.id !== udata.value.form.deleting.data?.id) {
      new_users.push(user)
    }
  })


  users.value = new_users
  localStorage.setItem("users", JSON.stringify(users.value))
  udata.value.form.deleting.dialog_model= false
}

</script>

<template>
  <div class="p-4 bg-gray-100 h-screen">
    <div class="flex justify-between items-center">
      <span class="mt-4 text-lg">
        Foydalanuvchilar jadvali
      </span>

      <button class="border px-2 pb-1 rounded-md cursor-pointer bg-green-400 text-white" @click="showDialog('create')">
        Yaratish
      </button>
    </div>
    <UITable :columns="cols" :rows="users" class="mt-4">
      <template #actions="props">
        <button class="border px-2 pb-1 rounded-md cursor-pointer bg-orange-400 text-white"
          @click="showDialog('edit', props)">
          Taxrirlash
        </button>
        <button class="border px-2 pb-1 rounded-md cursor-pointer bg-red-400 text-white"
          @click="showDeleteUserDialog(props)">
          O'chirish
        </button>
      </template>
    </UITable>
    <UIDialog v-model="udata.form.dialog_model" :dialogTitle="udata.form.dialog_title">
      <template #body>
        <div class="mt-4">
          <UIInput v-model="udata.form.name" placeholder="Ism" />
          <UIInput v-model="udata.form.surname" placeholder="Familya" class="mt-2" />
          <UIInput v-model="udata.form.email" placeholder="Pochta" class="mt-2" />
          <UIInput v-model="udata.form.phone_number" placeholder="Telefon raqam" class="mt-2" />
          <UIInput v-model="udata.form.address" placeholder="Manzil" class="mt-2" />
        </div>
      </template>

      <template #footer>
        <div class="flex items-center justify-end mt-4 gap-2">
          <button class="border px-2 pb-1 rounded-md cursor-pointer bg-green-400 text-white"
            v-if="udata.form.editing.value" @click="updateUserData">
            O'zgartirish
          </button>
          <button class="border px-2 pb-1 rounded-md cursor-pointer bg-green-400 text-white" @click="createNewUser"
            v-else>
            Yratish
          </button>
          <button class="border px-2 pb-1 rounded-md cursor-pointer bg-red-400 text-white"
            @click="udata.form.dialog_model = false">
            Bekor qilish
          </button>
        </div>
      </template>
    </UIDialog>

    <UIDialog v-model="udata.form.deleting.dialog_model" :dialogTitle="udata.form.deleting.dialog_title">
      <template #body>
        <div class="mt-4">
          <div class="text-lg">
            <span class="font-bold">{{ udata.form.deleting.data?.name }}</span>ni o'chirishni tasdiqlaysizmi?
          </div>
        </div>
      </template>

      <template #footer>
        <div class="flex items-center justify-end mt-4 gap-2">
          <button class="border px-2 pb-1 rounded-md cursor-pointer bg-green-400 text-white" @click="deleteUser">
            Tasdiqlash
          </button>
          <button class="border px-2 pb-1 rounded-md cursor-pointer bg-red-400 text-white"
            @click="udata.form.deleting.dialog_model = false">
            Bekor qilish
          </button>
      </div>
    </template>
  </UIDialog>
</div></template>