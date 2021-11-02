<template>
  <a-card
    :bordered="false"
    class="header-solid h-full"
    :bodyStyle="{ padding: 0 }"
  >
    <template #title>
      <div style="display:flex;justify-content: space-between">
        <h5 class="font-semibold m-0">Lista Estudiantes</h5>
      
        <a-input-search
          placeholder="Buscar"
          style="width: 200px"         
          v-model="search"
        />
      </div>
    </template>
    <a-table
      :columns="columns"
      :data-source="ListaFiltro"
      :pagination="true"
      :row-key="(ListaFiltro) => ListaFiltro.id_estudiante"
    >     
    </a-table>
  </a-card>
</template>

<script>
export default {
  props: {
    data: {
      type: Array,
      default: () => [],
    },
    columns: {
      type: Array,
      default: () => [],
    },
    cantidad:{
        type:Number,
        default:0
    }
  },
  data() {
    return {
      // Active button for the "Authors" table's card header radio button group.
      authorsHeaderBtns: "all",
      search:'',
    };
  },
  computed: {
     ListaFiltro(){
        return this.data.filter(item => {
            return item.nombres.toLowerCase().includes(this.search.toLowerCase())
        })
    }
  },
  methods: {
    Ver(id) {
      //this.$router.push({ name: "UsuarioEdit" ,params:{ id_usuario:id}});
    },
    Eliminar(id) {
      //this.$emit('Eliminar',id);
    },
    onChange(eve, id_estudiante) {
      //this.$emit("EnviarAsistencia", eve, id_estudiante);
    },    
  },
};
</script>
