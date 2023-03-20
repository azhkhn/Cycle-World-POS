<template>
  <v-row justify="center">
    <v-dialog v-model="customerDialog" max-width="600px">
      <v-card>
        <v-card-title>
          <span class="headline primary--text">{{ __('Customer Info') }}</span>
        </v-card-title>
        <v-card-text class="pa-0">
          <v-container>
            <v-row>
              <v-col cols="12">
                <v-text-field
                  dense
                  color="primary"
                  :label="frappe._('Customer Name')"
                  background-color="white"
                  hide-details
                  v-model="customer_info.customer_name"
                  @change="set_customer_info('customer_name', $event)"
                ></v-text-field>
              </v-col>
              <v-col cols="6">
                <v-text-field
                  dense
                  color="primary"
                  :label="frappe._('Email')"
                  background-color="white"
                  hide-details
                  v-model="customer_info.email_id"
                  @change="set_customer_info('email_id', $event)"
                ></v-text-field>
              </v-col>
              <v-col cols="6">
                <v-text-field
                  dense
                  color="primary"
                  :label="frappe._('Mobile No')"
                  background-color="white"
                  hide-details
                  v-model="customer_info.mobile_no"
                  @change="set_customer_info('mobile_no', $event)"
                ></v-text-field>
              </v-col>


              <v-col cols="6">
                <v-text-field
                  dense
                  color="primary"
                  :label="frappe._('GSTIN')"
                  background-color="white"
                  hide-details
                  v-model="customer_info.gstin"
                  @change="gstin_onchange('gstin', $event)"
                ></v-text-field>
              </v-col>
              <v-col cols="6">
                <v-text-field
                  dense
                  color="primary"
                  :label="frappe._('Address Line 1')"
                  background-color="white"
                  hide-details
                  v-model="customer_info.address_line1"
                  @change="set_customer_info('address_line1', $event)"
                ></v-text-field>
              </v-col>
              <v-col cols="6">
                <v-text-field
                  dense
                  color="primary"
                  :label="frappe._('Address Line 2')"
                  background-color="white"
                  hide-details
                  v-model="customer_info.address_line2"
                  @change="set_customer_info('address_line2', $event)"
                ></v-text-field>
              </v-col>
              <v-col cols="6">
                <v-text-field
                  dense
                  color="primary"
                  :label="frappe._('City')"
                  background-color="white"
                  hide-details
                  v-model="customer_info.city"
                  @change="set_customer_info('city', $event)"
                ></v-text-field>
              </v-col>
              <v-col cols="6">
                <v-autocomplete
                  clearable
                  dense
                  auto-select-first
                  color="primary"
                  :label="frappe._('GST State')"
                  v-model="customer_info.gst_state"
                  :items="states"
                  background-color="white"
                  :no-data-text="__('State not found')"
                  hide-details
                  @change="gst_state_onchange('gst_state', $event)"
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
                  :disabled = "true"
                  v-model="customer_info.gst_state_number"
                  @change="set_customer_info('gst_state_number', $event)"
                ></v-text-field>
              </v-col>
              <v-col cols="6">
                <v-text-field
                  dense
                  color="primary"
                  :label="frappe._('Postal Code')"
                  background-color="white"
                  hide-details
                  v-model="customer_info.pincode"
                  @change="set_customer_info('pincode', $event)"
                ></v-text-field>
              </v-col>
            </v-row>
          </v-container>
        </v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn color="error" dark @click="close_dialog">Close</v-btn>
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
    customer_info: '',
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
       number_state_mapping : {
          '35': 'Andaman and Nicobar Islands', 
          '37': 'Andhra Pradesh', 
          '12': 'Arunachal Pradesh', 
          '18': 'Assam', 
          '10': 'Bihar', 
          '04': 'Chandigarh', 
          '22': 'Chhattisgarh', 
          '26': 'Dadra and Nagar Haveli and Daman and Diu', 
          '07': 'Delhi', 
          '30': 'Goa', 
          '24': 'Gujarat', 
          '06': 'Haryana', 
          '02': 'Himachal Pradesh', 
          '01': 'Jammu and Kashmir', 
          '20': 'Jharkhand', 
          '29': 'Karnataka', 
          '32': 'Kerala', 
          '38': 'Ladakh', 
          '31': 'Lakshadweep Islands', 
          '23': 'Madhya Pradesh', 
          '27': 'Maharashtra', 
          '14': 'Manipur', 
          '17': 'Meghalaya', 
          '15': 'Mizoram', 
          '13': 'Nagaland', 
          '21': 'Odisha', 
          '97': 'Other Territory', 
          '34': 'Pondicherry', 
          '03': 'Punjab', 
          '08': 'Rajasthan', 
          '11': 'Sikkim', 
          '33': 'Tamil Nadu', 
          '36': 'Telangana', 
          '16': 'Tripura', 
          '09': 'Uttar Pradesh', 
          '05': 'Uttarakhand', 
          '19': 'West Bengal'
        },
    address_line1_old:"",
    city_old:"",
    call:true
  }),

  watch: {
    customer() {
      this.fetch_customer_details();
    },
    customer_info:
        { 
          handler(newval, oldval){
          this.address_line1_old = oldval==''?newval.address_line1:oldval.address_line1
          this.city_old = oldval==''?newval.city:oldval.city
          },
          deep: true
    }
  },

  methods: {
    gst_state_number_onchange(fieldname, $event){

      if(this.customer_info. gst_state_number){
        this.customer_info.gst_state = this.number_state_mapping[this.customer_info.gst_state_number]
      }
      else{
        this.customer_info.gst_state = ''
      }
    },
    gst_state_onchange(fieldname, $event){
      if(this.customer_info.gst_state){
        this.customer_info.gst_state_number = this.state_numbers[this.customer_info.gst_state]
      }
      else{
        this.customer_info.gst_state_number = ''
      }
      console.log('gst_state_number_onchange')
      this.set_customer_info(fieldname, $event)
    },
    gstin_onchange(fieldname, $event){
      this.customer_info.gst_state_number = this.customer_info.gstin.slice(0, 2)
      this.gst_state_number_onchange('gst_state_number', $event)
      this.set_customer_info(fieldname, $event)
    },
    close_dialog() {
      this.customerDialog = false;
    },

    set_customer_info(field, value) {
      const vm = this;
      // if(!this.call){
      //   console.log('in if')
      //   this.call = !this.call
      //   return
      // }
      frappe.call({
        method: 'posawesome.posawesome.api.posapp.set_customer_info',
        args: {
          fieldname: field,
          customer: this.customer_info.name,
          value: value,
          customer_info: vm.customer_info
        },
        callback: (r) => {
          if (!r.exc) {
            evntBus.$emit('update_customer_list')
            vm.customer_info[field] = value;
            evntBus.$emit('show_mesage', {
              text: __('Customer contact updated successfully.'),
              color: 'success',
            });
            frappe.utils.play_sound('submit');
          }
          else if(r.message){
            // vm.call=false
            // vm.customer_info[r.message] = vm[r.message+'_old']
          }
        },
      });
    },
  },
  created: function () {
    evntBus.$on('open_edit_customer', () => {
      this.customerDialog = true;
    });
    evntBus.$on('set_customer_info_to_edit', (data) => {
      this.customer_info = data;
    });
  },
};
</script>
