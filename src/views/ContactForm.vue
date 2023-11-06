<template>
  <Fieldset legend="Formulário de Contato">
    <form class="grid p-fluid" @submit.prevent="send">
      <div class="field col-6">
        <label for="nomecontato">Nome do Contato: </label>
        <InputText
          id="nomecontato"
          placeholder="Digite o nome do contato"
          required
          v-model="obj.name"
        />
      </div>
      <div class="field col-6">
        <label>Numero Telefone: </label>
        <InputMask
          mask="(99) 99999-9999"
          placeholder="Digite o numero do telefone"
          v-model="obj.fone"
          required
        />
      </div>
      <div class="field col-6">
        <label>Numero do CPF: </label>
        <InputMask
          mask="999.999.999-99"
          placeholder="Digite o numero do CPF"
          required
          v-model="obj.cpf"
        />
      </div>
      <div class="field col-6">
        <label>Numero do RG: </label>
        <InputText
          placeholder="Digite o numero do telefone"
          v-model="obj.rg"
          required
        />
      </div>

      <div class="field col-12">
        <hr />
      </div>
      <div class="field col-2">
        <Button
          label="Voltar"
          class="p-button-secondary"
          @click="back"
        ></Button>
      </div>
      <div class="field col-2">
        <Button label="Enviar" type="submit"></Button>
      </div>
    </form>
  </Fieldset>
</template>

<script>
//Models
import Contact from "../models/contact";

//Services
import ContactService from "@/service/contact_service";

export default {
  data() {
    return {
      obj: new Contact(),
      service: new ContactService(),
    };
  },
  mounted() {
    const json = sessionStorage.getItem("contact");
    this.obj = JSON.parse(json);
  },
  methods: {
    send() {
      if (this.obj.id == null) {
        this.service.create(this.obj).then(() => {
          this.$toast.add({
            severity: "success",
            summary: "Alerta de Sucesso.",
            detail: "Criado com sucesso.",
            life: 3000,
          });
          this.back();
        });
      } else {
        this.service.update(this.obj).then(() => {
          this.$toast.add({
            severity: "success",
            summary: "Alerta de Sucesso.",
            detail: "Atualizado com sucesso.",
            life: 3000,
          });
          this.back();
        });
      }
    },
    back() {
      this.$router.push("/");
    },
  },
};
</script>

<style></style>
