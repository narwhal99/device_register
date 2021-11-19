<template>
  <div>
    <v-card>
      <v-card-title>
        Device register
        <v-spacer />
        <v-col>
          <v-select label="Type" single-line hide-details> </v-select>
        </v-col>
        <v-col>
          <v-text-field v-model="search" label="Value" single-line hide-details>
          </v-text-field>
        </v-col>
      </v-card-title>
      <v-data-table
        :headers="headers"
        :items="item"
        hide-default-footer
        @click:row="handleClick"
      ></v-data-table>
    </v-card>
    <v-dialog v-model="showDialog" fullscreen persistent>
      <v-card>
        <v-card-title>
          <v-col>
            <v-row justify="center">
              <v-col cols="12" sm="6" md="3">
                <v-select
                  style="max-width: 200px"
                  outlined
                  hide-details
                  label="Status"
                  :items="status_select"
                  v-model="focusItem.status"
                ></v-select>
              </v-col>
              <v-spacer />
              <v-col cols="12" sm="6" md="3">
                <v-row no-gutters>
                  <v-col>
                    <v-btn outlined @click="closeDialog" color="success"
                      >Save</v-btn
                    >
                  </v-col>
                  <v-col>
                    <v-btn outlined @click="closeDialog">Close</v-btn>
                  </v-col>
                </v-row>
              </v-col>
            </v-row>
          </v-col>
        </v-card-title>
        <v-card-text>
          <v-row justify="center">
            <v-col cols="12" sm="6" md="3" v-if="focusItem.who_have_it">
              <v-text-field
                label="email"
                v-model="focusItem.who_have_it.email"
              ></v-text-field>
              <v-text-field
                label="Last name"
                v-model="focusItem.who_have_it.last"
              ></v-text-field>
              <v-text-field
                label="First name"
                v-model="focusItem.who_have_it.first"
              ></v-text-field>
              <v-dialog
                ref="dialog"
                v-model="modal"
                :return-value.sync="focusItem.issued"
                persistent
                width="290px"
              >
                <template v-slot:activator="{ on, attrs }">
                  <v-text-field
                    v-model="focusItem.issued"
                    label="Kiadva"
                    prepend-icon="mdi-calendar"
                    readonly
                    v-bind="attrs"
                    v-on="on"
                  ></v-text-field>
                </template>
                <v-date-picker v-model="focusItem.issued" scrollable>
                  <v-spacer></v-spacer>
                  <v-btn text color="primary" @click="modal = false">
                    Cancel
                  </v-btn>
                  <v-btn
                    text
                    color="primary"
                    @click="$refs.dialog.save(focusItem.issued)"
                  >
                    OK
                  </v-btn>
                </v-date-picker>
              </v-dialog>
            </v-col>
            <v-col cols="12" sm="6" md="3">
              <v-select
                label="Device type"
                :items="device_types"
                v-model="focusItem.device"
              ></v-select>
              <v-text-field label="ID" v-model="focusItem.id"></v-text-field>
              <v-text-field
                label="Type"
                v-model="focusItem.type"
              ></v-text-field>
              <v-dialog
                ref="dialog"
                v-model="modal"
                :return-value.sync="focusItem.in_stock_date"
                persistent
                width="290px"
              >
                <template v-slot:activator="{ on, attrs }">
                  <v-text-field
                    v-model="focusItem.in_stock_date"
                    label="In stock date"
                    prepend-icon="mdi-calendar"
                    readonly
                    v-bind="attrs"
                    v-on="on"
                  ></v-text-field>
                </template>
                <v-date-picker v-model="focusItem.in_stock_date" scrollable>
                  <v-spacer></v-spacer>
                  <v-btn text color="primary" @click="modal = false">
                    Cancel
                  </v-btn>
                  <v-btn
                    text
                    color="primary"
                    @click="$refs.dialog.save(focusItem.in_stock_date)"
                  >
                    OK
                  </v-btn>
                </v-date-picker>
              </v-dialog>
            </v-col>
          </v-row>
        </v-card-text>
      </v-card>
    </v-dialog>
  </div>
</template>

<script>
export default {
  data() {
    return {
      device_types: ["Laptop", "Mobile", "SIM"],
      modal: false,
      status_select: ["Kiadva", "Raktáron", "Rendelés alatt"],
      showDialog: false,
      focusItem: {},
      search: "",
      item: [
        {
          status: "Kiadva",
          id: "1237182371973981732",
          device: "Laptop",
          type: "Dell 123asd321",
          in_stock_date: "2021-05-05",
          who_have_it: {
            email: "molnar.andrej@damit.hu",
            first: "Andrej",
            last: "Molnár",
          },
          issued: "2021-10-11",
        },
        {
          status: "Raktáron",
          id: "1823189738127398",
          device: "Laptop",
          type: "HP 8u21asdio",
          in_stock_date: "2021-06-17",
          who_have_it: {
            email: "",
            first: "",
            last: "",
          },
          issued: "",
        },
        {
          status: "Kiadva",
          id: "1371298378129397812",
          device: "Mobile",
          type: "Xiaomi 9",
          in_stock_date: "2020-05-12",
          who_have_it: {
            email: "nagy.david@damit.hu",
            first: "Dávid",
            last: "Nagy",
          },
          issued: "2020-10-11",
        },
      ],
      headers: [
        { text: "Status", value: "status" },
        { text: "Who have it", value: "who_have_it.email" },
        { text: "Device", value: "device" },
        { text: "Type", value: "type" },
        { text: "Kiadva", value: "issued" },
        { text: "In stock date", value: "in_stock_date" },
        { text: "ID", value: "id" },
      ],
    };
  },
  methods: {
    handleClick(value) {
      this.showDialog = true;
      this.focusItem = value;
    },
    closeDialog() {
      this.showDialog = false;
      this.focusItem = {};
    },
  },
};
</script>