<template>
  <v-container>
    <v-row justify="center" class="mt-5">
      <v-col cols="9">
        <v-alert border="left" type="info" dense elevation="5">
          yield*
        </v-alert>

        <v-row>
          <v-col cols="6">
            <v-autocomplete
              chips
              dense
              clearable
              deletable-chips
              multiple
              small-chips
              v-model="values"
              :items="items"
              hide-selected
              solo-inverted
              :filter="customFilter"
            ></v-autocomplete>
          </v-col>
          <v-spacer />
          <v-col>
            <v-btn type="info" @click="addItem">Add</v-btn>
            <v-btn
              type="info"
              @click="modItem"
              :disabled="!selectedItem"
              elevation="2"
              >Modify</v-btn
            >
          </v-col>
        </v-row>

        <v-row>
          <v-simple-table style="width: 80%" dense>
            <template v-slot:default>
              <thead>
                <tr>
                  <th class="text-left" style="width: 20%">Category1</th>
                  <th class="text-left" style="width: 20%">Category2</th>
                  <th class="text-left" style="width: 30%">BIN</th>
                  <th class="text-left" style="width: 20%">REMOVE</th>
                </tr>
              </thead>
              <tbody>
                <tr
                  v-for="(item, index) in groups"
                  :key="index"
                  @click="selectItem(item)"
                  :class="{ selectedRow: item === selectedItem }"
                >
                  <td></td>
                  <td></td>
                  <td>{{ item.join(',') }}</td>
                  <td>
                    <v-icon small @click.prevent="removeItem(item)"
                      >mdi-close</v-icon
                    >
                  </td>
                </tr>
              </tbody>
            </template>
          </v-simple-table>
        </v-row>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
export default {
  name: 'YieldStar',
  data: () => ({
    values: [],
    items: [...Array(10)].map((_, i) => `B${i.toString().padStart(3, '0')}`),
    groups: [],
    selectedItem: null,
    desserts: [
      {
        name: 'Frozen Yogurt',
        calories: 159,
      },
      {
        name: 'Ice cream sandwich',
        calories: 237,
      },
      {
        name: 'Eclair',
        calories: 262,
      },
      {
        name: 'Cupcake',
        calories: 305,
      },
    ],
  }),
  methods: {
    logger() {
      console.log('logger');
    },
    addItem() {
      this.values.sort();
      this.groups.push([...this.values]);
      this.values.splice(0, this.values.length);
    },
    modItem() {
      this.selectedItem = this.values;
    },

    removeItem(row) {
      this.groups = this.groups.filter((r) => r !== row);
    },

    selectItem(row) {
      this.selectedItem = row;
      this.values = [...row];
    },

    customFilter: (item, queryText, itemText) => {
      const tokens = queryText.match(/B(\d+)\s?~\s?B(\d+)/i);
      if (tokens) {
        const number = parseInt(itemText.slice(1));
        return number >= tokens[1] && number <= tokens[2] ? true : false;
      }
      return (
        itemText.toLocaleLowerCase().indexOf(queryText.toLocaleLowerCase()) > -1
      );
    },
  },
  mounted() {},
};
</script>

<style scoped>
.selectedRow {
  font-weight: bold;
  color: darkslategrey;
}
</style>
