<template>
  <ion-page>
    <ion-header>
      <ion-toolbar color="primary">
        <ion-buttons slot="start">
          <ion-back-button default-href="/"></ion-back-button>
        </ion-buttons>
        <ion-title>List: {{title}}</ion-title>
      </ion-toolbar>
    </ion-header>
    <ion-content padding>
      <ion-list>
        <ion-item v-for="(entry, index) in entries" :key="index">
          <font-awesome-icon icon="tag" style="color:#3880ff; margin-right: 35px;" />
          <ion-label>
            <h2>{{entry.postenname}}</h2>
            <ion-note>Amount: {{entry.anzahl}}x</ion-note>
          </ion-label>
          <ion-button  fill="clear" size="large" slot="end" @click="removeEntry(entry)">
            <font-awesome-icon icon="trash" style="color:#666;" />
          </ion-button>
        </ion-item>
      </ion-list>
      <ion-fab vertical="bottom" horizontal="end" slot="fixed">
        <ion-fab-button @click="addEntry">
          <font-awesome-icon icon="plus" />
        </ion-fab-button>
      </ion-fab>
    </ion-content>
  </ion-page>
</template>

<script>
export default {
  name: "SingleListView",
  mounted() {
    this.refreshEntries();
  },
  data() {
    return {
      entries: []
    };
  },
  props: ["listid", "title"],
  methods: {
    async refreshEntries() {
      this.showLoading();
      let response = await this.$api.getEntries(this.listid);
      this.entries = response.data;
      this.hideLoading();
    },
    async removeEntry(entry) {
      this.showLoading();
      let response = await this.$api.removeEntry(entry);
      if (response.status === 204) {
        this.refreshEntries();
        this.presentToast("Löschen erfolgreich.");
      } else {
        this.presentAlert(
          "Fehler",
          "Leider ist beim Löschen etwas schief gegangen."
        );
      }
    },
    addEntry() {
      this.$router.replace({
        name: "new-entry",
        params: { listid: this.listid, title: this.title }
      });
    }
  }
};
</script>


<style scoped>
</style>