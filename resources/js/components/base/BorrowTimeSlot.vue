<template>
  <table class="timeslot">
    <caption>{{ title }}</caption>
    <tbody>
      <tr>
        <td>&nbsp;</td>
        <th v-for="day in days" :key="day">{{ day }}</th>
      </tr>
      <tr v-for="(time, timeIndex) in times" :key="time">
        <th>{{ time }}</th>
        <td v-for="(day, dayIndex) in days" :key="day">
          <label v-if="isAvailable(dayIndex, timeIndex)" :class="{selected: isSelected(dayIndex, timeIndex)}">
            {{ isSelected(dayIndex, timeIndex) ? 'Selected' : 'Available' }}
            <input type="checkbox" :checked="isSelected(dayIndex, timeIndex)" @click="select(dayIndex, timeIndex)">
          </label>
        </td>
      </tr>
    </tbody>
  </table>
</template>

<script>
export default {
  name: 'BorrowTimeSlot',
  props: {
    title: {
      type: String,
      required: true
    },
    availableSlots: {
      type: Array,
      required: true
    }
  },
  data () {
    return {
      days: ['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun'],
      times: ['8.00 - 10.00', '10.00 - 12.00', '12.00 - 14.00', '14.00 - 16.00', '16.00 - 18.00', '18.00 - 20.00'],
      selectedSlots: []
    }
  },
  computed: {
  },
  methods: {
    isAvailable (day, timeslot) {
      const selectedIndex = (day * 7) + timeslot
      return this.availableSlots.includes(selectedIndex)
    },
    isSelected (day, timeslot) {
      const selectedIndex = (day * 7) + timeslot
      return this.selectedSlots.includes(selectedIndex)
    },
    select (day, timeslot) {
      const selectedSlot = (day * 7) + timeslot

      if (this.mode === 'lender') {
        this.selectForLender(selectedSlot)
      } else {
        this.selectedSlots.push(selectedSlot)
        this.$emit('select', this.selectedSlots)
      }
    },
  }
}
</script>

<style lang="scss">
.timeslot {
  width: 100%;
  padding:2rem;

  caption {
    font-size: 2rem;
    font-weight: bold;
  }

  th{
    text-transform: uppercase;
  }

  tr{
    height:40px;
    margin-bottom:5px;
    text-align: center;
  }

  label {
    padding: 0.25rem 1rem;
    border:1px solid #999;
    border-radius:0.25rem;
    cursor: pointer;

    &.selected{
      background-color: cadetblue;
    }
  }

  input {
    display: none;
  }
}
</style>
