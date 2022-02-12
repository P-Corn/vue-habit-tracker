<template>
  <v-form @submit.prevent>
    <v-row 
      align="baseline"
      class="justify-end"
    >
      <v-col
        cols="12"
        sm="6"
      >
        <v-text-field 
          solo
          v-model="habitToAdd"
          placeholder="e.g. Program"
          maxLength="25"
          counter="25"
        />
      </v-col>
      <v-col
        cols="12"
        sm="6"
        class="d-flex"
        :class="$vuetify.breakpoint.xs ? 'justify-end' : 'justify-start'"
      >
        <v-btn
          color="primary"
          @click="$emit('habit-added', habitToAdd), habitToAdd=''"
          type="submit"
        >
          add habit
        </v-btn>
      </v-col>
    </v-row>
  </v-form>
</template>

<script>

export default {
  name: 'AddHabitForm',
  data: () => ({
    habitToAdd: ''
  })
};
</script>