<template>
    <el-form ref="formRef" style="max-width: 100%" :model="formulario" :rules="rulesForm" label-width="auto"
      :size="formSize" status-icon>
      <el-form-item label="Nombre" prop="nombre">
        <el-input v-model="formulario.nombre" />
      </el-form-item>
      <el-form-item label="Precio" prop="Precio">
        <el-input v-model="formulario.Precio" />
      </el-form-item>
      <el-form-item label="Descripcion" prop="descripcion">
        <el-select v-model="formulario.descripcion" placeholder="Ingrese la descripcion">
          <el-option v-for="Prodcutos in Productos" :key="producto.id" :label="Productos.nombre" :value="producto.id" />
  
        </el-select>
      </el-form-item>
  
    </el-form>
  </template>
  
  <script setup>
  
  import { onMounted, reactive, ref, watch } from 'vue'
  import Productos from './Productos.vue';
import Productos from './Productos.vue';
  
  const propiedad = defineProps({
    producto: {
      type: Array,
      required: true,
    },
    dataValue: Object,
  });
  
  const formSize = ref('default')
  const formRef = ref()
  const formulario = reactive({
    nombre: '',
    precio: '',
    descripcion: '',
  
  })
  
  const datosFormulario = () => {
  
    formulario.nombre = propiedad.dataValue[0].nombre;
    formulario.precio = propiedad.dataValue[0].precio;
    formulario.descripcion = propiedad.dataValue[0].descripcion;
   
  
  }
  
  const rulesForm = reactive({
    nombre: [
      { required: true, message: 'Por favor ingrese el nombre', trigger: 'blur' }
    ],
    precio: [
      {
        required: true,
        message: 'Ingrese el apellido',
        trigger: 'blur',
      },
    ],
    descripcion: [
      {
        required: true,
        message: 'Ingrese la descripcion',
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