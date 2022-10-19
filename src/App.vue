<script setup lang="ts">
import { reactive } from 'vue'
import { useVuelidate } from '@vuelidate/core'
import { required, email, helpers } from '@vuelidate/validators'
import { computed } from '@vue/reactivity';


function handleSubmit() {
  v$.value.$validate()

  if (v$.value.$errors.length > 0) return;


  console.log(state.name)
  console.log(state.email)
}

const state = reactive({
  name: '',
  email: '',
})

const rules = computed(() => {
  return {
    name: { required: helpers.withMessage('Nome é obrigatório', required) },
    email: { 
      required: helpers.withMessage('E-mail é obrigatório', required), 
      email: helpers.withMessage('Digite um e-mail válido', email)
    }
  }
})


const v$ = useVuelidate(rules, state)

console.log(v$)

</script>

<template>
  <form @submit.prevent="handleSubmit">
    <h2>Cadastro</h2>
    <div class="form-group">
      <label for="name">Nome</label>
      <input type="text" id="name" v-model="state.name">

      <div class="input-errors" v-for="error in v$.name.$errors" :key="error.$uid">
        <div class="error-msg">
          {{ error.$message }}
        </div>
      </div>
    </div>

    <div class="form-group">
      <label for="email">E-mail</label>
      <input type="text" id="email" v-model="state.email">


      <div class="input-errors" v-for="error in v$.email.$errors" :key="error.$uid">
        <div class="error-msg">
          {{ error.$message }}
        </div>
      </div>
    </div>

    <button type="submit">Submit</button>
  </form>
</template>

<style scoped>


h2 {
  text-align: left;
  font-size: 1.5rem;
  color: #010101
}

.form-group {
  display: flex;
  flex-direction: column;
  align-items: flex-start;

  margin-top: 0.5rem;
}

.form-group label {
  font-size: 0.875rem;
}

.form-group input {
  padding: 0.5rem 1rem;
  font-size: 1rem;
  border-radius: 4px;
  border: 1px solid #ccc;
}

.form-group input:focus {
  outline: none;
  box-shadow: 0 0 0 2px #2196F3;
}

.input-errors {
  margin-top: 0.25rem;
  font-size: 0.875rem;
  color: #ff5500
}

button[type="submit"] {
  background-color: #2196F3;
  width: 100%;
  margin-top: 1rem;
  color: #fff;

  transition: background-color 150ms linear;
}

button[type="submit"]:focus {
  outline: none;
  box-shadow: 0 0 0 2px #2196F3;
}

button[type="submit"]:hover {
  background-color: #4aa5ef;
}
</style>
