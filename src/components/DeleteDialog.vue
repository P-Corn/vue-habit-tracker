<template>
  <v-row justify="center">
    <v-dialog
      persistent
      max-width="290"
      v-model="dialogOpen"
    >
      <v-card>
        <v-card-title class="text-h5">
          Are you sure you want to delete this habit?
        </v-card-title>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn
            color="primary darken-1"
            text
            @click="$emit('delete-canceled')"
          >
            No
          </v-btn>
          <v-btn
            color="primary darken-1"
            text
            @click="$emit('habit-deleted')"
          >
            Yes
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </v-row>
</template>

<script>

export default {
  name: 'DeleteDialog',
  props: {
    dialogOpen: Boolean
  }
};
</script>