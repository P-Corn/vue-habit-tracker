<template>
  <v-app>
    <v-main>
      <v-container>
        <h1>Habit Tracker</h1>
        <v-divider class="my-6"/>
        <AddHabitForm 
          @habit-added="addHabit"
        />
        <v-spacer class="my-8"></v-spacer>
        <DataTable 
          :habits="habits"
          :datesList="datesList"
          @date-checked="checkOffDate"
          @delete-pressed="[dialogOpen = true, habitToDelete = $event]"
        />
        <h3 
          v-if="habits.length === 0"
          class="mt-5"
        >Start by adding a habit!</h3>
      </v-container>
      <DeleteDialog 
        :dialogOpen="dialogOpen"
        @habit-deleted="deleteHabit"
        @delete-canceled="[dialogOpen = false, habitToDelete = null]"
      />
    </v-main>
  </v-app>
</template>

<script>
import dayjs from 'dayjs';
const customParseFormat = require('dayjs/plugin/customParseFormat');
dayjs.extend(customParseFormat);
import DataTable from './components/DataTable.vue';
import AddHabitForm from './components/AddHabitForm.vue';
import DeleteDialog from './components/DeleteDialog.vue';

export default {
  name: 'App',

  components: {
    DataTable,
    AddHabitForm,
    DeleteDialog
  },

  mounted () {
    this.populateDates();
    // set state from local storage
    if (localStorage.getItem('habits'))
      this.habits = JSON.parse(localStorage.getItem('habits'));
    this.updateHabitDates();
  },

  data: () => ({
    habits: [],
    datesList: [],
    dialogOpen: false,
    habitToDelete: null
  }),

  methods: {
    addHabit: function (habitToAdd) {
      // Check if input is empty or if habit already exists
      if (habitToAdd === '' || this.habits.some(habit => habit.title.toLowerCase() === habitToAdd.toLowerCase()))
        return;
      let completionDates = [];
      // create object for each date in datesList
      for (let date of this.datesList)
        completionDates.push({ date, checked: false });
      this.habits.push({ title: habitToAdd, completionDates });
      this.updateLocalStorage();
    },

    deleteHabit: function () {
      this.habits.splice(this.habitToDelete, 1);
      this.habitToDelete = null;
      this.dialogOpen = false;
      this.updateLocalStorage();
    },

    checkOffDate: function (indices) {
      const { habitIndex, completionDateIndex, checked } = indices;
      this.habits[habitIndex].completionDates[completionDateIndex].checked = !checked;
      this.updateLocalStorage();
    },

    populateDates: function () {
      // Add dates from today to 7 days ago
      for (let i = 0; i < 7; i++)
        this.datesList.push(dayjs().subtract(i, 'day').format('ddd D'));
    },

    updateHabitDates: function () {
      for (let habit of this.habits) {
        // Get index of current habit
        const habitIndex = this.habits.findIndex(item => item === habit);
        // Reformat string date to dayjs object
        let formattedDate = dayjs(habit.completionDates[0].date, 'dddD');
        // calculate the spread between today and most recent day in habit.completionDates.date
        const daySpread = dayjs().diff(formattedDate, 'day');
        if (daySpread > 0) {
          for (let i = daySpread; i > 0; i--) {
            // add new date at the beginning of array
            this.habits[habitIndex].completionDates.unshift({ 
              date: formattedDate.add(1, 'day').format('ddd D'), 
              checked: false 
            });
            // pop off last index to maintain an array of 7 items
            this.habits[habitIndex].completionDates.pop();
            // the formatted needs to be updated to reflect the new date added on each iteration
            formattedDate = dayjs(habit.completionDates[0].date, 'dddD');
          }
        } else return;
      }
      this.updateLocalStorage();
    },

    updateLocalStorage: function () { localStorage.setItem('habits', JSON.stringify(this.habits)); }
  }
};
</script>