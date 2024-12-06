<template>
  <el-form ref="formRef" style="max-width: 100%" :model="formulario" :rules="rulesForm" label-width="auto"
    :size="formSize" status-icon>
    <el-form-item label="Nombre" prop="nombre">
      <el-input v-model="formulario.nombre" />
    </el-form-item>
    <el-form-item label="Apellido" prop="Apellido">
      <el-input v-model="formulario.Apellido" />
    </el-form-item>
    <el-form-item label="Documento" prop="documento">
      <el-select v-model="formulario.documento" placeholder="Ingrese su documento">
        <el-option v-for="Clientes in Clientes" :key="cliente.id" :label="cliente.nombre" :value="cliente.id" />

      </el-select>
    </el-form-item>

  </el-form>
</template>

<script setup>

import { onMounted, reactive, ref, watch } from 'vue'
import Personas from './Clientes.vue';

const propiedad = defineProps({
  persona: {
    type: Array,
    required: true,
  },
  dataValue: Object,
});

const formSize = ref('default')
const formRef = ref()
const formulario = reactive({
  nombre: '',
  apellido: '',
  documento: '',

})

const datosFormulario = () => {

  formulario.nombre = propiedad.dataValue[0].nombre;
  formulario.apellido = propiedad.dataValue[0].apellido;
  formulario.documento = propiedad.dataValue[0].documento;
 

}

const rulesForm = reactive({
  nombre: [
    { required: true, message: 'Por favor ingrese el nombre', trigger: 'blur' }
  ],
  apellido: [
    {
      required: true,
      message: 'Ingrese el apellido',
      trigger: 'blur',
    },
  ],
  documento: [
    {
      required: true,
      message: 'Ingrese su documento',
      trigger: 'blur',
    },
  ],
})

const limpiarFormulario = () => {
  formRef.value.resetFields()
}

const validarFormulario = () => {

  return new Promise((resolve) => {
    formRef.value?.validate((valid) => {
      if (valid) {
              resolve(true)
            } else {
                resolve(false)             
            }
            
        })
        })        

    
}

watch(
  () => propiedad.dataValue,
  (newData) => {
    datosFormulario();
  }
);

defineExpose({validarFormulario,formulario,limpiarFormulario})



</script>




<style scoped></style>