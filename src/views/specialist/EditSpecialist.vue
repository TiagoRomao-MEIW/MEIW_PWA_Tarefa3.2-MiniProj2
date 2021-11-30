<template>
  <section class="page-section">
     <b-container>
      <HeaderPage title="Editar Especialista"/>
   
      <!--FORM-->
      <b-row>
        <b-col cols="2"></b-col>
        <b-col cols="8">
          <form @submit.prevent="update">
            <div class="form-group">
              <input
                v-model="specialist.name"
                type="text"
                class="form-control form-control-lg"
                id="txtName"
                placeholder="escreve nome especialista"
                required
              />
            </div>
            <div class="form-group">
              <textarea
                id="txtDescription"
                class="form-control form-control-lg"
                placeholder="escreve animais"
                cols="30"
                rows="10"
                v-model="specialist.animals"
                required
              ></textarea>
            </div>

            <button type="submit" class="btn btn-outline-success btn-lg mr-2">
              <i class="fas fa-edit"></i> GRAVAR</button>
            <router-link
              :to="{name: 'listSpecialists'}"
              tag="button"
              class="btn btn-outline-danger btn-lg"
            ><i class="fas fa-window-close"></i> VOLTAR</router-link>
          </form>
        </b-col>
        <b-col cols="2"></b-col>
      </b-row>
     </b-container>
  </section>
</template>

<script>
import { EDIT_SPECIALIST } from "@/store/specialists/specialist.constants";
import HeaderPage from "@/components/HeaderPage.vue"
import router from "@/router";
import { mapGetters } from "vuex";

export default {
  name: "EditSpecialist",
  components: {
    HeaderPage
  },
  data: () => {
    return {
      specialist: {}
    };
  },
  computed: {
    ...mapGetters("specialists", ["getSpecialistsById", "getMessage"])
  },
  methods: {
    update() {
      this.$store.dispatch(`specialist/${EDIT_SPECIALIST}`, this.$data.specialist).then(
        () => {
          this.$alert(
            this.getMessage,
            "Utilizador atualizado!",
            "success"
          );
          router.push({name: 'listSpecialists'});
        },
        err => {
          this.$alert(`${err.message}`, "Erro", "error");
        }
      );
    }
  },
  created() {
    this.specialist = this.getSpecialistsById(this.$route.params.specialistId);
  }
};
</script>
