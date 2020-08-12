<template>
    <v-toolbar-title>
        {{title}}
        <v-btn icon @click="openDialog"><v-icon>mdi-pencil</v-icon></v-btn>
        <v-dialog v-model="dialog" max-width="400">
          <v-card>
            <v-card-title>
              제목 수정
              <v-spacer/>
              <v-btn @click="save" icon>
                <v-icon>mdi-content-save</v-icon>
              </v-btn>
              <v-btn @click="dialog=false" icon>
                <v-icon>mdi-close</v-icon>
              </v-btn>
            </v-card-title>
            <v-card-text>
              <v-text-field v-model="text" outlined label="제목" counter="25" @keypress.enter="save" hide-details="true"></v-text-field>
            </v-card-text>
          </v-card>
        </v-dialog>
    </v-toolbar-title>
</template>

<script>
export default {
  props: ['title'],
  data: function () {
    return {
      dialog: false,
      text: this.title
    }
  },
  methods: {
    openDialog () {
      this.dialog = true
    },
    async save () {
      try {
        await this.$firebase.database().ref().child('site').update({ title: this.text })
      } finally {
        this.dialog = false
      }
    }

  }
}
</script>
