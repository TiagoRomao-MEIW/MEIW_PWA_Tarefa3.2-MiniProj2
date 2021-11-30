<template>
  <section class="page-section">
    <b-container>
      <HeaderPage title="Gestão de Especialistas"/>
      <!--MENU DE TOPO-->
      <b-row class="mb-4">
        <b-col cols="1"></b-col>
        <b-col>
          <router-link
            :to="{name:'addSpecialist'}"
            tag="button"
            class="btn btn-outline-success mr-2 mt-2"
          ><i class="fas fa-plus-square"></i> ADICIONAR ESPECIALISTA</router-link>
                <router-link
                    :to="{name:'editSpecialist'}"
                    tag="button"
                    class="btn btn-outline-success mr-2 mt-2"
                ><i class="fas fa-edit"></i> EDITAR ESPECIALISTA</router-link>
                  <button
                    @click="viewSpecialist(specialist._id)"
                    type="button"
                    class="btn btn-outline-warning mr-2 mt-2"
                  ><i class="fas fa-search"></i> VER</button>
                  <button
                    @click="removeSpecialist(specialist._id)"
                    type="button"
                    class="btn btn-outline-danger mr-2 mt-2"
                  ><i class="fas fa-trash-alt"></i> REMOVER</button>
          <router-link
            :to="{name:'admin'}"
            tag="button"
            class="btn btn-outline-info mr-2 mt-2"
          ><i class="fas fa-bars"></i> MENU PRINCIPAL</router-link>
        </b-col>
        <b-col cols="1"></b-col>
      </b-row>
      
      <!--TABLE-->
      <b-row>
        <b-col cols="1"></b-col>
        <b-col>
          <table class="table table-striped">
            <thead class="thead-dark">
              <tr>
                <th scope="col">
                  ESPECIALISTA
                  <i class="fas fa-arrow-up" v-if="sortType===1" @click="sort()"></i>
                  <i class="fas fa-arrow-down" v-else  @click="sort()"></i>                
                  </th>
                <th scope="col">ANIMAIS</th>
                <th scope="col">AÇÕES</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="specialist of specialists" :key="specialist._id">
                <td  class="pt-4">{{specialist.specialist}}</td>
                <td class="pt-4">{{specialist.animals}}</td>
                <td>
                  <router-link
                    :to="{name:'editSpecialist', params:{specialistId: specialist._id}}"
                    tag="button"
                    class="btn btn-outline-success mr-2"
                  ><i class="fas fa-edit"></i> EDITAR</router-link>
                  <button
                    @click="viewSpecialist(specialist._id)"
                    type="button"
                    class="btn btn-outline-warning mr-2"
                  ><i class="fas fa-search"></i> VER</button>
                  <button
                    @click="removeSpecialist(specialist._id)"
                    type="button"
                    class="btn btn-outline-danger mr-2"
                  ><i class="fas fa-trash-alt"></i> REMOVER</button>
                </td>
              </tr>
            </tbody>
          </table>
        </b-col>
          <b-col cols="1"></b-col>
      </b-row>
    </b-container>
  </section>
</template>

<script>
import {
  FETCH_SPECIALISTS,
  REMOVE_SPECIALISTS //falta colocar no store
} from "@/store/specialists/specialist.constants";
import HeaderPage from "@/components/HeaderPage.vue"
import { mapGetters } from "vuex";


export default {
  name: "ListSpecialists",
   components: {
    HeaderPage
  },
  data: function() {
    return {
      specialists: [],
      sortType: 1
    };
  },
  computed: {
    ...mapGetters("specialist", ["getSpecialists","getMessage"])
  },
  methods: {
    fetchSpecialists() {
      this.$store.dispatch(`specialist/${FETCH_SPECIALISTS}`).then(
        () => {
          this.speciatlists = this.getSpecialists;
        },
        err => {
          this.$alert(`${err.message}`, "Erro", "error");
        });
    },
    sort() {
      this.specialists.sort(this.compareNames)
      this.sortType *= -1      
    },
    compareNames(u1,u2) {
      if(u1.specialist > u2.specialist) return 1 * this.sortType
      else if(u1.specialist < u2.specialist) return -1 * this.sortType
      else return 0
    },
    viewSpecialist(id) {
      const specialist = this.specialists.find(specialist => specialist._id === id);
      this.$fire({
        title: specialist.specialist,
        html: <p><b>Animais:</b> ${this.specialist.animals}</p>
        /* imageUrl: user.name,
        imageWidth: 400,
        imageHeight: 200,
        imageAlt: "Imagem desconhecida" */
      });
    },/* 
    generateTemplate(specialist) {
      let response = `
          <p><b>Aniamais:</b> ${this.specialist.animals}</p>`          
          
      return response;
    }, */
    removeSpecialist(id) {
      this.$confirm(
        "Se sim, clique em OK",
        "Deseja mesmo remover o patrocinador?",
        "warning",
        { confirmButtonText: "OK", cancelButtonText: "Cancelar" }
      ).then(
        () => {
          this.$store.dispatch(`specialist/${REMOVE_SPECIALISTS}`, id).then(() => {
            this.$alert(
              this.getMessage,
              "Patrocinador removido!",
              "success"
            );
            this.fetchSpecialists();
          });
        },
        () => {
          this.$alert("Remoção cancelada!", "Informação", "info");
        }
      );
    }
  },
  created() {
    this.fetchSpecialists();
  }
};
</script>
<style>
.green {
  color: green
}

.red {
  color: red
}
</style>