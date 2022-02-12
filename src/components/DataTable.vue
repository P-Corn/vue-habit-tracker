<template>
  <div>
    <template>
      <v-simple-table>
        <template>
          <thead>
            <tr>
              <th></th>
              <th
                v-for="(date, index) in datesList"
                :key="index"
              >
                {{ date }}
              </th>
            </tr>
          </thead>
          <tbody>
            <tr
              v-for="(habit, habitIndex) in habits"
              :key="habitIndex"
            >
              <td>
                <v-btn
                  icon
                  color="red darken-1"
                  @click="$emit('delete-pressed', habitIndex)"
                >
                  <v-icon>
                    mdi-delete
                  </v-icon>
                </v-btn>
                <span>{{ habit.title }}</span>
              </td>
              <td
                v-for="(item, completionDateIndex) in habit.completionDates"
                :key="completionDateIndex"
              >
                <v-checkbox
                  :input-value="item.checked"
                  @click="$emit('date-checked', { habitIndex, completionDateIndex, checked: item.checked })"
                />
              </td>
            </tr>
          </tbody>
        </template>
      </v-simple-table>
    </template>
  </div>
</template>

<script>

export default {
  name: 'DataTable',
  props: {
    habits: Array,
    datesList: Array
  }
};
</script>

<style scoped>
  th, td {
    white-space: nowrap;
  }
</style>