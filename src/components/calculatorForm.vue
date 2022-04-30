<template>
  <v-container>
    <v-form v-model="valid" ref="form">
      <v-row class="text-center pt-4">
        <v-col cols="12">
          <v-select
            v-model="numberOfPeople"
            required
            outlined
            :items="selectItems"
            hide-details="auto"
            label="تعداد نفرات"
            >
          </v-select>
        </v-col>

        <v-col
          v-if="numberOfPeople"
          v-for="(item, index) in numberOfPeople"
          :cols="12">
          <v-text-field
            v-model="viewModel.participation[index].price"
            required
            hide-details="auto"
            outlined
            :rules="requiredRule"
            type="number"
            suffix="تومان"
            :label="`میزان مشارکت نفر ${item}`"
            >
          </v-text-field>
        </v-col>
        <v-col
          :cols="12">
          <v-text-field
            v-if="numberOfPeople"
            v-model="viewModel.cost"
            required
            outlined
            :rules="requiredRule"
            type="number"
            hide-details="auto"
            suffix="تومان"
            label="هزینه های انجام شده"
            >
          </v-text-field>
        </v-col>
        <v-col
          :cols="12">
          <v-text-field
            v-if="numberOfPeople"
            v-model="viewModel.selledPrice"
            required
            outlined
            :rules="requiredRule"
            hide-details="auto"
            type="number"
            suffix="تومان"
            label="قیمت فروش"
            >
          </v-text-field>
        </v-col>
        <v-col
          :cols="12">
          <v-btn
            v-if="numberOfPeople"
            @click="calculateBenefit"
            block
            outlined
            color="primary"
            >
            محاسبه سود
          </v-btn>
        </v-col>
        <v-dialog
          v-model="dialog"
          fullscreen
          hide-overlay
          transition="dialog-bottom-transition"
          >
          <v-card>
            <v-toolbar
              dark
              color="primary"
              >
              <v-btn
                icon
                dark
                @click="dialog = false"
                >
                <v-icon>mdi-close</v-icon>
              </v-btn>
              <v-toolbar-title>محاسبه سود</v-toolbar-title>
              <v-spacer></v-spacer>
              <v-toolbar-items v-if="false">
                <v-btn
                  dark
                  text
                  @click="dialog = false"
                  >
                  Save
                </v-btn>
              </v-toolbar-items>
            </v-toolbar>
            <v-list
              subheader
              >
              <v-subheader>سود محاسبه شده</v-subheader>
              <v-simple-table>
                <template v-slot:default>
                  <thead>
                    <tr>
                      <th class="text-right">
                        نفر
                      </th>
                      <th class="text-right">
                        سود
                      </th>
                    </tr>
                  </thead>
                  <tbody>
                    <tr
                      v-for="(item, index) in viewModel.participation"
                      :key="index"
                      v-if="item.price"
                      >
                      <td>{{ index + 1 }}</td>
                      <td>{{ item.benefit }} تومان</td>
                    </tr>
                  </tbody>
                </template>
              </v-simple-table>
            </v-list>
            <v-divider></v-divider>
          </v-card>
        </v-dialog>
      </v-row>
    </v-form>
  </v-container>
</template>

<script>
export default {
  name: 'calculatorForm',
  methods: {
    calculateBenefit () {
      if (this.$refs.form.validate()) {
        let totalPaidValue = 0
        this.viewModel.participation.forEach((i) => {
          if (i.price !== null) {
            totalPaidValue = parseInt(i.price) + parseInt(totalPaidValue)
          }
        })
        totalPaidValue = totalPaidValue
        const totalBenefit = parseInt(this.viewModel.selledPrice) - parseInt(this.viewModel.cost) - parseInt(totalPaidValue)
        if (totalBenefit > 0) {
          this.viewModel.participation.forEach((i, index) => {
            if (i.price !== null) {
              this.viewModel.participation[index].percent = parseInt(i.price) / parseInt(totalPaidValue)
            }
          })
          this.viewModel.participation.forEach((i, index) => {
            if (i.price !== null) {
              i.benefit = i.percent * totalBenefit
            }
          })
        }
        this.dialog = true
      }
    }
  },
  data () {
    return {
      dialog: false,
      valid: false,
      requiredRule: [
        v => !!v || 'الزامی'
      ],
      numberOfPeople: null,
      viewModel: {
        participation: [
          {
            price: null,
            percent: null
          },
          {
            price: null,
            percent: null
          },
          {
            price: null,
            percent: null
          },
          {
            price: null,
            percent: null
          },
          {
            price: null,
            percent: null
          }
        ],
        cost: null,
        selledPrice: null
      },
      result: [],
      selectItems: [
        {
          text: '1',
          value: 1
        },
        {
          text: '2',
          value: 2
        },
        {
          text: '3',
          value: 3
        },
        {
          text: '4',
          value: 4
        },
        {
          text: '5',
          value: 5
        },
      ],
    }
  }
}
</script>
