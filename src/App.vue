<template>
  <div>
    <div class="input-container no-print">
      <div class="title">
        Birthdate
      </div>
      <label for="year">Year</label>
      <input type="number" v-model="dates.birth.year" name="year" />
      <label for="month">Month</label>
      <input type="number" v-model="dates.birth.month" name="month" />
      <label for="day">Day</label>
      <input type="number" v-model="dates.birth.day" name="day" />
    </div>
    <div class="input-container no-print">
      <div class="title">
        Life Expectancy
      </div>
      <label for="year">Year</label>
      <input type="number" v-model="dates.expectancy.year" name="year" />
      <label for="month">Month</label>
      <input type="number" v-model="dates.expectancy.month" name="month" />
      <label for="day">Day</label>
      <input type="number" v-model="dates.expectancy.day" name="day" />
    </div>
    <div class="btn-container no-print">
      <button class="btn" @click="renderBlocks">
        Calculate
      </button>
    </div>
    <table>
      <tr>
        <th
          style="width: 10px; height:5px"
          v-for="i in boxes.width + 1"
          :key="i"
        >
          {{ i - 1 }}
        </th>
      </tr>
      <tr style="width: 10px; height:5px" v-for="i in boxes.height" :key="i">
        <th>
          {{ i }}
        </th>
        <td
          style="width: 10px; height:5px"
          v-for="j in boxes.width"
          :key="j"
          :ref="`${i},${j}`"
        >&#x2612;</td>
      </tr>
    </table>
  </div>
</template>

<script>
export default {
  methods: {
    weeksBetween: function(week1, week2) {
      return Math.round((week1 - week2) / (7 * 24 * 60 * 60 * 1000));
    },
    yearsBetween: function(year1, year2) {
      var ageDifMs = year1 - year2;
      var ageDate = new Date(ageDifMs);
      return Math.abs(ageDate.getUTCFullYear() - 1970);
    },
    clearBoard: function() {
      for (let i = 1; i <= this.boxes.width; i++) {
        for (let j = 1; j <= this.boxes.height; j++) {
          this.$refs[`${j},${i}`][0].innerHTML = "&#x2612;";
        }
      }
    },
    renderBlocks: function() {
      this.clearBoard();
      for (
        let i = 1;
        i <=
        this.weeksBetween(
          new Date(
            this.dates.expectancy.year,
            this.dates.expectancy.month,
            this.dates.expectancy.day
          ),
          new Date(
            this.dates.birth.year,
            this.dates.birth.month,
            this.dates.birth.day
          )
        ) %
          52;
        i++
      ) {
        for (
          let j = 1;
          j <=
          this.yearsBetween(
            new Date(
              this.dates.expectancy.year,
              this.dates.expectancy.month,
              this.dates.expectancy.day
            ),
            new Date(
              this.dates.birth.year,
              this.dates.birth.month,
              this.dates.birth.day
            )
          );
          j++
        ) {
          this.$refs[`${j},${i}`][0].innerHTML = "&#x2610;";
        }
      }
      for (
        let i = 1;
        i <=
        this.weeksBetween(
          new Date(),
          new Date(
            this.dates.birth.year,
            this.dates.birth.month,
            this.dates.birth.day
          )
        ) %
          52;
        i++
      ) {
        for (
          let j = 1;
          j <=
          this.yearsBetween(
            new Date(),
            new Date(
              this.dates.birth.year,
              this.dates.birth.month,
              this.dates.birth.day
            )
          );
          j++
        ) {
          this.$refs[`${j},${i}`][0].innerHTML = "&#x25A0;";
        }
      }
    },
  },
  data: function() {
    return {
      dates: {
        birth: {
          month: 3,
          day: 1,
          year: 1996,
        },
        expectancy: {
          month: 1,
          day: 1,
          year: 2025,
        },
      },
      boxes: {
        height: 90,
        width: 52,
      },
    };
  },
};
</script>

<style>
.input-container {
  padding: 12px;
  margin: 12px 0px;
  border: 1px rgba(0, 0, 0, 0.1) solid;
  border-radius: 4px;
}

.input-container input {
  margin: 0px 12px;
  padding: 3px 2px;
  border: 1px rgba(0, 0, 0, 0.1) solid;
}

.input-container .title {
  font-size: 24px;
  margin-bottom: 6px;
}

.btn-container {
  margin: 16px 0px;
}

.btn {
  background-color: whitesmoke;
  border: 1px rgba(0, 0, 0, 0.1) solid;
  border-radius: 4px;
  padding: 6px 12px;
}

@media print {
  .no-print,
  .no-print * {
    display: none !important;
  }
}
</style>
