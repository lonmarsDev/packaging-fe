
<template>
  <div class="container">
    <date-range-picker v-model="dateRange" :v-bind="startDate" :locale-data="locale">
      <!--Optional scope for the input displaying the dates -->
      <div slot="input">Created date</div>
    </date-range-picker>

    <v-server-table :columns="columns" :options="options"></v-server-table>
  </div>
</template>

<script>
import DateRangePicker from "vue2-daterange-picker";

const moment = require("moment");
export default {
  components: { DateRangePicker },
  data() {
    return {
      dateRange: {
        startDate: "2017-09-05",
        endDate: new Date()
      },
      startDate: "2017-09-05",
      endDate: "2017-09-15",
      locale: {
        direction: "ltr", //direction of text
        format: "DD-MM-YYYY", //fomart of the dates displayed
        separator: " - ", //separator between the two ranges
        applyLabel: "Apply",
        cancelLabel: "Cancel",
        weekLabel: "W",
        customRangeLabel: "Custom Range",
        daysOfWeek: moment.weekdaysMin(), //array of days - see moment documenations for details
        monthNames: moment.monthsShort(), //array of month names - see moment documenations for details
        firstDay: 1 //ISO first day of week - see moment documenations for details
      },

      columns: [
        "order_name",
        "customer_company",
        "customer_name",
        "created_at",
        "delivered_amount",
        "totalAmount"
      ],
      options: {
        headings: {
          order_name: "Order name",
          customer_company: "Customer Company",
          customer_name: "Customer Name",
          created_at: "Order date",
          delivered_date: "Delivered Amount",
          totalAmount: "total Amount"
        },

        filterable: true,
        //filterByColumn: true,
        perPageValues: [],
        perPage: 5,
        dateColumns: [moment("created_at")],
        dateFormat: "MM/DD/YYYY",
        requestFunction: function(data) {
          return this.axios
            .get(process.env.apiURL, {
              params: data
            })
            .catch(
              function(e) {
                this.dispatch("error", e);
              }.bind(this)
            );
        },

        responseAdapter: function(resp) {
          return {
            data: resp.data.data,
            count: resp.data.count
          };
        },

        customFilters: ["created_at"]
      }
    };
  }
};
</script>

<style>
#app {
  width: 95%;
  margin-top: 50px;
}

.VuePagination {
  text-align: center;
}

.vue-title {
  text-align: center;
  margin-bottom: 10px;
}

.vue-pagination-ad {
  text-align: center;
}

.glyphicon.glyphicon-eye-open {
  width: 16px;
  display: block;
  margin: 0 auto;
}

th:nth-child(3) {
  text-align: center;
}

.VueTables__child-row-toggler {
  width: 16px;
  height: 16px;
  line-height: 16px;
  display: block;
  margin: auto;
  text-align: center;
}

.VueTables__child-row-toggler--closed::before {
  content: "+";
}

.VueTables__child-row-toggler--open::before {
  content: "-";
}
</style>