<template>
  <v-row justify="center" align="center">
    <v-col cols="12">
      <v-card>
        <v-toolbar color="transparent" elevation="0">
          <v-toolbar-title>List of Customers</v-toolbar-title>
          <v-spacer></v-spacer>
          <v-btn color="pink" @click="createdialog = true"><v-icon left>mdi-plus</v-icon> Create</v-btn>
        </v-toolbar>
        <v-divider></v-divider>
      </v-card>

      <v-data-table :headers="headers" :items="customers" :items-per-page="10" class="elevation-1" :loading="loading"
        loading-text="Loading... Pleae wait">
        <template v-slot:item.profilePicture="{ item }">
          <v-badge color="red" content="VIP" offset-x="10" offset-y="40" v-if="item.isVip == true">
            <v-avatar size="60">
              <v-img lazy-src="https://picsum.photos/id/11/10/6"
                :src="item.profilePicture"></v-img></v-avatar></v-badge>
          <div v-else>
            <v-avatar size="60">
              <v-img lazy-src="https://picsum.photos/id/11/10/6" :src="item.profilePicture"></v-img></v-avatar>
          </div>
        </template>
        <template v-slot:item.actions="{ item }">
          <v-btn color="pink" @click="getCustomerDetails(item)">View</v-btn>
        </template></v-data-table>
      <template>
        <div class="text-center">
          <v-dialog v-model="customerdialog" width="500">

            <v-card>
              <v-card-title class="text-h5 pink"><v-icon>mdi-information</v-icon> <v-spacer></v-spacer>
                Customer Details <v-spacer></v-spacer>
                <v-btn  icon @click="customerdialog = false" > <v-icon>mdi-close</v-icon></v-btn>

              </v-card-title>


              <v-row>
                <v-col cols="12" md="4"><v-img max-height="200" max-width="200" :src="customerPhoto"></v-img></v-col>
                <v-col cols="12" md="6">
                  <v-list-item two-line>
                    <v-list-item-content>
                      <v-list-item-title>Name</v-list-item-title>
                      <v-list-item-title>{{ customerName }}</v-list-item-title>
                      <v-spacer></v-spacer> <v-spacer></v-spacer> <v-spacer></v-spacer>
                      <v-spacer></v-spacer><v-spacer></v-spacer>
                      <v-list-item-title>Email</v-list-item-title>
                      <v-list-item-title>{{ customerEmail }}</v-list-item-title>
                      <v-spacer></v-spacer> <v-spacer></v-spacer> <v-spacer></v-spacer>
                      <v-spacer></v-spacer><v-spacer></v-spacer>
                      <v-list-item-title>Phone #</v-list-item-title>
                      <v-list-item-title>{{ customerPhone }}</v-list-item-title>
                      <v-spacer></v-spacer> <v-spacer></v-spacer> <v-spacer></v-spacer>
                      <v-spacer></v-spacer><v-spacer></v-spacer>
                      <v-list-item-title>Age</v-list-item-title>
                      <v-list-item-title>{{ customerAge }}</v-list-item-title>
                    </v-list-item-content>
                  </v-list-item>
                </v-col>

              </v-row>





              <v-divider></v-divider>

              <v-card-actions>
                <v-spacer></v-spacer>
                <v-btn color="primary" text @click="customerdialog = false">
                  I accept
                </v-btn>
              </v-card-actions>
            </v-card>
          </v-dialog>
          <v-dialog v-model="createdialog" width="500" >
            <v-card>
              <v-toolbar class="mb-10">
                <v-toolbar-title>
                  <v-btn color="transparent" @click="createdialog = true"><v-icon left>mdi-account</v-icon>
                    Create </v-btn> 
                </v-toolbar-title> <v-spacer></v-spacer>
                <v-icon @click="createdialog = false"> mdi-close</v-icon>
              </v-toolbar>
              <v-card-text>
                <v-form>
                  <v-text-field outlined label="Name"></v-text-field>
                  <v-text-field outlined label="Email Address"></v-text-field>
                  <v-text-field outlined label="Contact Number"></v-text-field>
                  <v-textarea label="Address" outlined rows="2"></v-textarea>
                  <v-text-field label="Age" outlined type="number"></v-text-field>
                  <v-checkbox label="is VIP"></v-checkbox>
                  <v-btn block @click="createdialog= false" color="primary" class="mb-5">Submit</v-btn>
                </v-form>
                <v-divider></v-divider>
              </v-card-text>
            </v-card>
          </v-dialog>
        </div>
      </template>
    </v-col>
  </v-row>
</template>


<script>
export default {
  name: 'IndexPage',
  data() {
    return {
      headers: [

        { text: 'Photo', value: 'profilePicture' },
        { text: 'Name', value: 'name' },
        { text: 'Email', value: 'email' },
        { text: 'Phone', value: 'phone' },
        { text: 'Address', value: 'address' },
        { text: 'Age', value: 'age' },
        { text: 'Status', value: 'isVip' },
        { text: '', value: "actions" },
      ],
      customers: [],
      loading: true,
      customerdialog: false,
      createdialog: false,
      submit:false,
      customerName: "",
      customerEmail: "",
      customerPhone: "",
      customerAge: 0,
      customerPhoto: "",


    };
  },
  methods: {
    getCustomers() {
      this.$axios.get('/customers')
        .then(response => {
          console.log(response.data)
          this.customers = response.data;
          this.loading = false
        })
        .catch(err => {
          console.log(err)
        });
    },
    getCustomerDetails(item) {
      console.log(item)

      this.customerName = item.name
      this.customerEmail = item.email
      this.customerPhone = item.phone
      this.customerAge = item.age
      this.customerPhoto = item.profilePicture
      this.customerdialog = true







    }
  },
  mounted() {
    this.getCustomers()
  }
}
</script>
