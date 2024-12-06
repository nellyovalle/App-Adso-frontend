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
          <el-option v-for="cargos in cargos" :key="cargos.id" :label="cargo.nombre" :value="cargo.id" />
  
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
    cargo: '',
   
  
  })
  
  const datosFormulario = () => {
  
    formulario.nombre = propiedad.dataValue[0].nombre;
    formulario.cargo = propiedad.dataValue[0].cargo;
    formulario.salario = propiedad.dataValue[0].salario;
   
  
  }
  
  const rulesForm = reactive({
    nombre: [
      { required: true, message: 'Por favor ingrese el nombre', trigger: 'blur' }
    ],
   cargo: [
      {
        required: true,
        message: 'Ingrese el apellido',
        trigger: 'blur',
      },
    ],
    salario: [
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