<template>
    <LayoutMain>
        <template #slotLayout>
            <Header :titulo="'Prodcutos'" :tituloBoton="'Crear Nuevo Producto'" :abrir="abrirFormulario" />


            <Formulario :titulo="'Clientes'" v-model:is-open="mostrarFormulario" :is-edit="editandoFormulario"
                @save="guardarDatos" @update="actualizarDatos">
                <template #slotForm>
                    <el-row :gutter="20">
                        <el-col :xs="24" :sm="24" :md="24" :lg="24" :xl="24">
                            <Form v-model:is-open="mostrarFormulario" :is-edit="editandoFormulario" ref="formRef"
                                :productos="FormProductos" :dataValue="dataProductosById" />
                        </el-col>
                    </el-row>
                </template>

            </Formulario>

            <el-table :data="crearProducto" stripe style="width: 100%">
                <el-table-column prop="nombre" label="nombre" />
                <el-table-column prop="precio" label="precio" />
                <el-table-column prop="descripcion" label="descripcion" />
                <el-table-column fixed="right" label="Acciones" min-width="120">
                    <template #default="registro">
                        <el-button link type="primary" size="large" :icon="Edit"
                            @click="editarFormulario(registro.row.id)">
                        </el-button>
                        <el-button link type="danger" :icon="Delete"
                            @click="eliminarProducto(registro.row.id)"></el-button>
                    </template>
                </el-table-column>
            </el-table>

        </template>


    </LayoutMain>
</template>


<script lang="ts" setup>
import { onMounted, reactive, ref, watch } from 'vue'
import LayoutMain from './form.Productos.vue';
import Formulario from './form.Productos.vue'
import Header from './form.Productos.vue'
import { Delete, Edit } from "@element-plus/icons-vue"
import FormClientes from './form.Productos.vue'
import { ElMessage, ElMessageBox } from 'element-plus'
import axios from 'axios';
import FormProductos from './form.Productos.vue';


const mostrarFormulario = ref(false)
const editandoFormulario = ref(false)
const formRef = ref()
const dataProductosById = ref()
const clientes = ref([])
const cargos = ref([])


const abrirFormulario = () => {
    mostrarFormulario.value = true
    editandoFormulario.value = false
}

const editarFormulario = async (id) => {
  
    mostrarFormulario.value = true
    editandoFormulario.value = true
}

const guardarDatos = async () => {
    const validacion = await formRef.value?.validarFormulario()
    if (validacion) {
        await crearProducto()
    }
}

const actualizarDatos = async () => {
    const validacion = await formRef.value?.validarFormulario()
    if (validacion) {
        await actualizarProducto()
    }
}

const crearProducto= async () => {

    const url =  'http://127.0.0.1:8000/api/producto/save'

    const dataFormulario = {
        nombre: formRef.value.formulario.nombre,
        precio: formRef.value.formulario.precio,
        descripcion: formRef.value.formulario.descripcion,
       
    }
    try {
        axios.post(url, dataFormulario)
            .then(function (response) {
                console.log(response);
                formRef.value?.limpiarFormulario()
                ElMessage({
                    message: 'El producto se creo con exito    .',
                    type: 'success',
                })
                datosProducto()
                mostrarFormulario.value = false

            })
            .catch(function (error) {
                console.log(error);
            });

    } catch (error) {
        console.error('error crear producto ', error)
    }
}

const actualizarProducto = async () => {
 
    const url = 'http://127.0.0.1:8000/api/producto/update'

    const dataFormulario = {
        id:dataProductosById.value[0].id,
        nombre: formRef.value.formulario.nombre,
        precio: formRef.value.formulario.precio,
        descripcion: formRef.value.formulario.descripcion,
    }
    try {
        axios.put(url, dataFormulario)
            .then(function (response) {
                console.log(response);
                formRef.value?.limpiarFormulario()
                ElMessage({
                    message: 'El producto se actualizo con exito    .',
                    type: 'success',
                })
                datosProducto()
                mostrarFormulario.value = false

            })
            .catch(function (error) {
                console.log(error);
            });

    } catch (error) {
        console.error('error crear producto', error)
    }

}


const eliminarProducto = async (id) => {

    const url = 'http://127.0.0.1:8000/api/producto/delete'

    ElMessageBox.confirm(
        'Esta seguro de eliminar el producto',
        'Eliminar Registro',
        {
            confirmButtonText: 'SI',
            cancelButtonText: 'Cancelar',
            type: 'error',
        }
    )
        .then(() => {

            try {
                axios.delete(url, { data: { id } })
                    .then(function (response) {
                        datosProducto()

                    })
                    .catch(function (error) {
                        console.log(error);
                    });

            } catch (error) {
                console.error('error crear producto ', error)
            }
            ElMessage({
                type: 'success',
                message: 'Se elimino correctamente el registro',
            })
        })
        .catch(() => {
            ElMessage({
                type: 'info',
                message: 'Eliminacion cancelada',
            })
        })

}
const datosProducto = async () => {

    const url = 'http://127.0.0.1:8000/api/producto/save'

    try {
        axios.get(url)
            .then(function (response) {
                cargos.value = response.data.result
               // console.log(response);

            })
            .catch(function (error) {
                console.log(error);
            });

    } catch (error) {
        console.error('error crear producto ', error)
    }


}
const getProductos = async () => {

    const url = 'http://127.0.0.1:8000/api/producto/datos'

    try {
        axios.get(url)
            .then(function (response) {
                clientes.value = response.data.result
               // console.log(response);

            })
            .catch(function (error) {
                console.log(error);
            });

    } catch (error) {
        console.error('error crear producto ', error)
    }




}

onMounted(() => {
    getProductos()
    datosProducto()
})

</script>