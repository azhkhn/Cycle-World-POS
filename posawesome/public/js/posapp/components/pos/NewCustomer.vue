<template>
  <v-row justify="center">
    <v-dialog v-model="customerDialog" max-width="600px">
      <v-card>
        <v-card-title>
          <span class="headline primary--text">{{ __('New Customer') }}</span>
        </v-card-title>
        <v-card-text class="pa-0">
          <v-container>
            <v-row>
              <v-col cols="6">
                <v-text-field
                  dense
                  color="primary"
                  :label="frappe._('Customer Name')"
                  background-color="white"
                  hide-details
                  v-model="customer_name"
                ></v-text-field>
              </v-col>
              <v-col cols="6">
                <v-text-field
                  dense
                  color="primary"
                  :label="frappe._('Mobile No')"
                  background-color="white"
                  hide-details
                  v-model="mobile_no"
                ></v-text-field>
              </v-col>
              <v-col cols="6">
                <v-text-field
                  dense
                  color="primary"
                  :label="frappe._('GSTIN')"
                  background-color="white"
                  hide-details
                  v-model="gstin"
                  @change="gstin_onchange()"
                ></v-text-field>
              </v-col>
              
              <v-col cols="6">
                <v-text-field
                  dense
                  color="primary"
                  :label="frappe._('Email Id')"
                  background-color="white"
                  hide-details
                  v-model="email_id"
                ></v-text-field>
              </v-col>
              <v-col cols="6">
                <v-text-field
                  dense
                  color="primary"
                  :label="frappe._('Address Line 1')"
                  background-color="white"
                  hide-details
                  v-model="address_line1"
                ></v-text-field>
              </v-col>
              <v-col cols="6">
                <v-text-field
                  dense
                  color="primary"
                  :label="frappe._('Address Line 2')"
                  background-color="white"
                  hide-details
                  v-model="address_line2"
                ></v-text-field>
              </v-col>
              <v-col cols="6">
                <v-text-field
                  dense
                  color="primary"
                  :label="frappe._('City')"
                  background-color="white"
                  hide-details
                  v-model="city"
                ></v-text-field>
              </v-col>
              <v-col cols="6">
                <v-autocomplete
                  clearable
                  dense
                  auto-select-first
                  color="primary"
                  :label="frappe._('GST State')"
                  v-model="gst_state"
                  :items="states"
                  background-color="white"
                  :no-data-text="__('State not found')"
                  hide-details
                  @change="gst_state_onchange()"
                >
                </v-autocomplete>
              </v-col>
              <v-col cols="6">
                <v-text-field
                  dense
                  color="primary"
                  :label="frappe._('GST State Number')"
                  background-color="white"
                  hide-details
                  v-model="gst_state_number"
                  :disabled = "true"
                ></v-text-field>
              </v-col>
              <v-col cols="6">
                <v-text-field
                  dense
                  color="primary"
                  :label="frappe._('Postal Code')"
                  background-color="white"
                  hide-details
                  v-model="pincode"
                ></v-text-field>
              </v-col>
              <v-col cols="6">
                <v-menu
                  ref="birthday_menu"
                  v-model="birthday_menu"
                  :close-on-content-click="false"
                  transition="scale-transition"
                  dense
                >
                  <template v-slot:activator="{ on, attrs }">
                    <v-text-field
                      v-model="birthday"
                      :label="frappe._('Birthday')"
                      readonly
                      dense
                      clearable
                      hide-details
                      v-bind="attrs"
                      v-on="on"
                      color="primary"
                    ></v-text-field>
                  </template>
                  <v-date-picker
                    v-model="birthday"
                    color="primary"
                    no-title
                    scrollable
                    :max="frappe.datetime.now_date()"
                    @input="birthday_menu = false"
                  >
                  </v-date-picker>
                </v-menu>
              </v-col>
              <v-col cols="6">
                <v-autocomplete
                  clearable
                  dense
                  auto-select-first
                  color="primary"
                  :label="frappe._('Customer Group')"
                  v-model="group"
                  :items="groups"
                  background-color="white"
                  :no-data-text="__('Group not found')"
                  hide-details
                >
                </v-autocomplete>
              </v-col>
              <v-col cols="6">
                <v-autocomplete
                  clearable
                  dense
                  auto-select-first
                  color="primary"
                  :label="frappe._('Territory')"
                  v-model="territory"
                  :items="territorys"
                  background-color="white"
                  :no-data-text="__('Territory not found')"
                  hide-details
                  append-icon="mdi-plus"
                  @click:append="new_territory"
                >
                </v-autocomplete>
              </v-col>
            </v-row>
          </v-container>
        </v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn color="error" dark @click="close_dialog">{{
            __('Close')
          }}</v-btn>
          <v-btn color="success" dark @click="submit_dialog">{{
            __('Submit')
          }}</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </v-row>
</template>

<script>
import { evntBus } from '../../bus';
export default {
  data: () => ({
    customerDialog: false,
    pos_profile: '',
    customer_name: '',
    gstin: '',
    mobile_no: '',
    email_id: '',
    referral_code: '',
    birthday: null,
    birthday_menu: false,
    group: '',
    groups: [],
    territory: '',
    address_line1:'',
    address_line2:'',
    city:'',
    state:'',
    gst_state_number:'',
    pincode:'',
    territorys: [],
    regex : /^\d+$/,
    states:[
        'Andaman and Nicobar Islands',
        'Andhra Pradesh',
        'Arunachal Pradesh',
        'Assam',
        'Bihar',
        'Chandigarh',
        'Chhattisgarh',
        'Dadra and Nagar Haveli and Daman and Diu',
        'Delhi',
        'Goa',
        'Gujarat',
        'Haryana',
        'Himachal Pradesh',
        'Jammu and Kashmir',
        'Jharkhand',
        'Karnataka',
        'Kerala',
        'Ladakh',
        'Lakshadweep Islands',
        'Madhya Pradesh',
        'Maharashtra',
        'Manipur',
        'Meghalaya',
        'Mizoram',
        'Nagaland',
        'Odisha',
        'Other Territory',
        'Pondicherry',
        'Punjab',
        'Rajasthan',
        'Sikkim',
        'Tamil Nadu',
        'Telangana',
        'Tripura',
        'Uttar Pradesh',
        'Uttarakhand',
        'West Bengal',
        ],
    state_numbers : {
        "Andaman and Nicobar Islands": "35",
        "Andhra Pradesh": "37",
        "Arunachal Pradesh": "12",
        "Assam": "18",
        "Bihar": "10",
        "Chandigarh": "04",
        "Chhattisgarh": "22",
        "Dadra and Nagar Haveli and Daman and Diu": "26",
        "Delhi": "07",
        "Goa": "30",
        "Gujarat": "24",
        "Haryana": "06",
        "Himachal Pradesh": "02",
        "Jammu and Kashmir": "01",
        "Jharkhand": "20",
        "Karnataka": "29",
        "Kerala": "32",
        "Ladakh": "38",
        "Lakshadweep Islands": "31",
        "Madhya Pradesh": "23",
        "Maharashtra": "27",
        "Manipur": "14",
        "Meghalaya": "17",
        "Mizoram": "15",
        "Nagaland": "13",
        "Odisha": "21",
        "Other Territory": "97",
        "Pondicherry": "34",
        "Punjab": "03",
        "Rajasthan": "08",
        "Sikkim": "11",
        "Tamil Nadu": "33",
        "Telangana": "36",
        "Tripura": "16",
        "Uttar Pradesh": "09",
        "Uttarakhand": "05",
        "West Bengal": "19",
       },
  }),
  watch: {},
  methods: {
    gst_state_onchange(){
      if(this.gst_state){
        this.gst_state_number = this.state_numbers[this.gst_state]
      }
      else{
        this.gst_state_number = ''
      }
    },
    gstin_onchange(){
      this.gst_state_number = this.gstin.slice(0, 2)
    },
    close_dialog() {
      this.customerDialog = false;
    },
    new_territory(){
      evntBus.$emit('open_new_territory')
    },
    getCustomerGroups() {
      if (this.groups.length > 0) return;
      const vm = this;
      frappe.db
        .get_list('Customer Group', {
          fields: ['name'],
          page_length: 1000,
          
        })
        .then((data) => {
          if (data.length > 0) {
            data.forEach((el) => {
              vm.groups.push(el.name);
            });
          }
        });
    },
    getCustomerTerritorys() {
      if (this.territorys.length > 0) return;
      const vm = this;
      frappe.db
        .get_list('Territory', {
          fields: ['name'],
          page_length: 1000,
          limit:999999,
        })
        .then((data) => {
          if (data.length > 0) {
            data.forEach((el) => {
              vm.territorys.push(el.name);
            });
          }
        });
    },
    submit_dialog() {
      if (this.customer_name) {
        const vm = this;
        const args = {
          customer_name: this.customer_name,
          company: this.pos_profile.company,
          gstin: this.gstin,
          mobile_no: this.mobile_no,
          email_id: this.email_id,
          referral_code: this.referral_code,
          birthday: this.birthday,
          customer_group: this.group,
          territory: this.territory,
          address_line1:this.address_line1,
          address_line2:this.address_line2,
          city:this.city,
          state:this.gst_state,
          gst_state_number:this.gst_state_number,
          pincode:this.pincode
        };
        frappe.call({
          method: 'posawesome.posawesome.api.posapp.create_customer',
          args: args,
          callback: (r) => {
            if (!r.exc && r.message.name) {
              evntBus.$emit('show_mesage', {
                text: __('Customer contact created successfully.'),
                color: 'success',
              });
              args.name = r.message.name;
              frappe.utils.play_sound('submit');
              evntBus.$emit('add_customer_to_list', args);
              evntBus.$emit('set_customer', r.message.name);
              vm.customer_name = '';
              vm.gstin = '';
              vm.mobile_no = '';
              vm.email_id = '';
              vm.referral_code = '';
              vm.birthday = '';
              vm.group = '';
              vm.address_line1 = '';
              vm.address_line2 = '';
              vm.city = '';
              vm.state = '';
              vm.gst_state = '';
              vm.gst_state_number = '';
              vm.pincode = '';
              vm.territory = '';
              vm.customerDialog = false;
            }
            else{
              vm.customerDialog = true;
            }
          },
        });
        // this.customerDialog = false;
      }
    },
  },
  created: function () {
    evntBus.$on('open_new_customer', (customer) => {
      this.customerDialog = true;
      if(this.regex.test(customer.srcElement.value)){
        this.mobile_no = customer.srcElement.value
      }
      else{
        this.customer_name = customer.srcElement.value?customer.srcElement.value:''
      }
      
    });
    evntBus.$on('register_pos_profile', (data) => {
      this.pos_profile = data.pos_profile;
    });
    evntBus.$on('add_territory_to_list', (data) => {
      this.territorys.push(data.name);
    });
    evntBus.$on('set_territory', (data) => {
      this.territory = data;
    });
    this.getCustomerGroups();
    this.getCustomerTerritorys();
  },
};
</script>
