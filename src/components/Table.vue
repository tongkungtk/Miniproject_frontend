<template>
  <div>
  <v-data-table
    :headers="headers"
    :items="employeeItem"
    sort-by="calories"
    class="elevation-1"
  >
    <template v-slot:top>
      <v-toolbar
        flat
      >
        <v-toolbar-title>จัดการข้อมูล</v-toolbar-title>
        <v-divider
          class="mx-4"
          inset
          vertical
        ></v-divider>
        <v-spacer>
          <v-btn
              color="primary"
              dark
              class="mb-2"
              @click="openDialog('add',defaultItem)"
            >
              เพิ่มข้อมูล
            </v-btn>
        </v-spacer>
      </v-toolbar>
    </template>
    <template v-slot:[`item.actions`]="{ item }">
      <v-btn small outlined @click="openDialog('edit', item)" color="blue">
      <v-icon>
        mdi-pencil
      </v-icon>
      </v-btn>
      
      <v-btn small outlined @click="deleteItem(item)" color="red" class="m1-2">
      <v-icon>
        mdi-delete
      </v-icon>
      </v-btn>
    </template>
    <template v-slot:no-data>
      <v-btn
        color="primary"
        @click="initialize"
      >
        Reset
      </v-btn>
    </template>
  </v-data-table>
  <v-dialog
          v-model="dialogCreate"
          max-width="500px"
        >
          <v-card>
            <v-card-title>
              <span class="text-h5">{{ formTitle }}</span>
            </v-card-title>

            <v-card-text>
              <v-container>
                <v-row>
                  <v-col
                    cols="12"
                    sm="6"
                    md="4"
                  >
                    <v-text-field
                      v-model="editedItem.name"
                      label="Dessert name"
                    ></v-text-field>
                  </v-col>
                  <v-col
                    cols="12"
                    sm="6"
                    md="4"
                  >
                    <v-text-field
                      v-model="editedItem.calories"
                      label="Calories"
                    ></v-text-field>
                  </v-col>
                  <v-col
                    cols="12"
                    sm="6"
                    md="4"
                  >
                    <v-text-field
                      v-model="editedItem.fat"
                      label="Fat (g)"
                    ></v-text-field>
                  </v-col>
                  <v-col
                    cols="12"
                    sm="6"
                    md="4"
                  >
                    <v-text-field
                      v-model="editedItem.carbs"
                      label="Carbs (g)"
                    ></v-text-field>
                  </v-col>
                  <v-col
                    cols="12"
                    sm="6"
                    md="4"
                  >
                    <v-text-field
                      v-model="editedItem.protein"
                      label="Protein (g)"
                    ></v-text-field>
                  </v-col>
                </v-row>
              </v-container>
            </v-card-text>

            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn
                color="blue darken-1"
                text
                @click="close"
              >
                Cancel
              </v-btn>
              <v-btn
                color="blue darken-1"
                text
                @click="save(formTitle)"
              >
                บันทึกข้อมูล
              </v-btn>
            </v-card-actions>
          </v-card>
        </v-dialog>
        <v-dialog v-model="dialogDelete" max-width="500px">
          <v-card>
            <v-card-title class="text-h5">คุณต้องการลบข้อมูลในตารางนี้ใช่ หรือ ไม่?</v-card-title>
            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn color="blue darken-1" text @click="closeDelete">ยกเลิก</v-btn>
              <v-btn color="blue darken-1" text @click="deleteItemConfirm">ตกลง</v-btn>
              <v-spacer></v-spacer>
            </v-card-actions>
          </v-card>
        </v-dialog>
</div>
</template>

<script>
export default {
  data: () => ({
    dialogCreate: false,
    dialogDelete: false,
    headers: [
      {
        text: 'Dessert (100g serving)',
        align: 'start',
        sortable: false,
        value: 'name'
      },
      { text: 'Calories', value: 'calories' },
      { text: 'Fat (g)', value: 'fat' },
      { text: 'Carbs (g)', value: 'carbs' },
      { text: 'Protein (g)', value: 'protein' },
      { text: 'Actions', value: 'actions', sortable: false }
    ],
    employeeItem: [],
    editedIndex: -1,
    editedItem: {
      name: '',
      calories: 0,
      fat: 0,
      carbs: 0,
      protein: 0
    },
    defaultItem: {
      name: '',
      calories: 0,
      fat: 0,
      carbs: 0,
      protein: 0
    },
    formTitle: ''
  }),

  watch: {
    dialog (val) {
      val || this.close()
    },
    dialogDelete (val) {
      val || this.closeDelete()
    }
  },

  created () {
    this.initialize();
  },

  methods: {
    async initialize () {
      this.employeeItem = []
      try {
        var data 
      } catch (error) {
        
      }
    },
    openDialog (status, item) {
      this.formTitle = ''
      if (status === 'add') {
        this.dialogCreate = true
        this.formTitle = 'เพิ่มข้อมูล'
        this.editedItem = item
        this.dialogCreate = true
      } else {
        this.formTitle = 'แก้ไขข้อมูล'
        this.editedIndex = this.desserts.indexOf(item)
        this.editedItem = item
        this.dialogCreate = true
      }
    },

    editItem (item) {
      this.editedIndex = this.desserts.indexOf(item)
      this.editedItem = Object.assign({}, item)
      this.dialog = true
    },

    deleteItem (item) {
      this.editedIndex = this.desserts.indexOf(item)
      this.editedItem = item
      this.dialogDelete = true
    },

    deleteItemConfirm () {
      this.desserts.splice(this.editedIndex, 1)
      this.closeDelete()
    },

    close() {
      this.dialogCreate = false;
      this.editedItem = {};
      this.defaultItem = {};
    },

    closeDelete() {
      this.dialogDelete = false;
      this.$nextTick(() => {
      this.editedItem = Object.assign({}, this.defaultItem);
      this.editedIndex = -1;
      });
    },

    save (action) {
      if (action === 'เพิ่มข้อมูล') {
        this.desserts.push(this.editedItem)
      } else {
        Object.assign(this.desserts[this.editedIndex], this.editedItem)
      }
      this.close()
    }
  }
}
</script>

<style>

</style>
