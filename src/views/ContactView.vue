<template>
  <div>
    <Fieldset legend="Gerenciamento de Contatos">
      <Toolbar>
        <template #start> <Button @click="goNew">Novo</Button> </template>
      </Toolbar>
      <DataTable :value="list">
        <Column field="id" header="id"></Column>
        <Column field="name" header="name"></Column>
        <Column field="fone" header="fone"></Column>
        <Column field="cpf" header="cpf"></Column>
        <Column field="rg" header="rg"></Column>
        <Column header="Ações">
          <template #body="slotProps">
            <Button
              icon="pi pi-pencil"
              class="p-button-rounded p-button-success mr-2"
              @click="showUpdate(slotProps.data)"
            />
            <Button
              icon="pi pi-trash"
              class="p-button-rounded p-button-warning"
              @click="showRemove(slotProps.data)"
            /> </template
        ></Column>
      </DataTable>
    </Fieldset>
  </div>
</template>

<script>
//Service
import ContactService from "../service/contact_service";

//Models
import Contact from "../models/contact";

export default {
  data() {
    return {
      obj: new Contact(),
      list: [],
      service: new ContactService(),
    };
  },
  mounted() {
    this.findAll();
  },
  methods: {
    findAll() {
      this.service.findAll().then((data) => {
        this.list = data;
      });
    },
    goNew() {
      this.obj = new Contact();
      sessionStorage.setItem("contact", JSON.stringify(this.obj));
      this.$router.push("/form");
    },
    showUpdate(data) {
      this.obj = data;
      sessionStorage.setItem("contact", JSON.stringify(this.obj));
      this.$router.push("/form");
    },
    showRemove(data) {
      this.obj = data;
      this.$confirm.require({
        header: "Deseja excluir?",
        message: "Tem certeza que deseja excluir este registro?",
        acceptLabel: "Confirmar",
        rejectLabel: "Cancelar",
        accept: () => {
          this.service
            .delete(this.obj.id)
            .then(() => {
              this.$toast.add({
                severity: "success",
                summary: "Alerta de Sucesso.",
                detail: "Deletado com sucesso.",
                life: 3000,
              });
              this.findAll();
            })
            .catch((err) => {
              console.error(err);
            });
        },
      });
    },
  },
};
</script>

<style></style>
