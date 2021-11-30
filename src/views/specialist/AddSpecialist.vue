<template>
  
  <section class="page-section">
    <b-container>
      <HeaderPage title="Adicionar Especialista"/>
     
      <!--FORM-->
      <b-row>
        <b-col cols="2"></b-col>
        <b-col cols="8">
          <form @submit.prevent="add">
            <div class="form-group">
              <input
                v-model="name"
                type="text"
                class="form-control form-control-lg"
                id="txtName"
                placeholder="Inserir nome especialista"
                required
              />
            </div>
            <div class="form-group">
              <textarea
                id="txtDescription"
                class="form-control form-control-lg"
                placeholder="Adicionar animais"
                cols="30"
                rows="10"
                v-model="animals"
              ></textarea>
            </div>
            <button type="submit" class="btn btn-outline-success btn-lg mr-2">
              <i class="fas fa-plus-square"></i> GRAVAR
            </button>
            <router-link
              :to="{name: 'listSpecialists'}"
              tag="button"
              class="btn btn-outline-danger btn-lg"
            >
            <i class="fas fa-window-close"></i> VOLTAR
            </router-link>
          </form>
        </b-col>
        <b-col cols="2"></b-col>
      </b-row>
    </b-container>
  </section>
</template>

<script>
import { ADD_SPECIALIST } from "@/store/specialists/specialist.constants";
import HeaderPage from "@/components/HeaderPage.vue"
import router from "@/router";
import { mapGetters } from "vuex";

export default {
  name: "AddSpecialist",
   components: {
    HeaderPage
  },
  data: () => {
    return {
      name: "",
      animals: ""
    };
  },
  computed: {
     ...mapGetters("specialist", ["getMessage"]),
  },
  methods: {
    add() {
        this.$store.dispatch(`specialist/${ADD_SPECIALIST}`, this.$data).then(
          () => {
            this.$alert(
              this.getMessage,
              "Utilizador adicionado!",
              "success"
            );
            router.push({name: 'listSpecialists'});
          }, 
          err => {
            this.$alert(`${err.message}`, "Erro", "error");
          }
        );
      }
    }
  };
</script>