<template>
    <v-row justify="center">
      <v-dialog v-model="territoryDialog" max-width="600px">
        <v-card>
          <v-card-title>
            <span class="headline primary--text">{{ __('New Territory') }}</span>
          </v-card-title>
          <v-card-text class="pa-0">
            <v-container>
              <v-row>
                <v-col cols="6">
                  <v-text-field
                    dense
                    color="primary"
                    :label="frappe._('Territory Name')"
                    background-color="white"
                    hide-details
                    v-model="territory_name"
                  ></v-text-field>
                </v-col>
                <v-col cols="6">
                  <v-autocomplete
    clearable
    dense
    auto-select-first
    color="primary"
    :label="frappe._('Parent Territory')"
    v-model="parent_territory"
    :items="parent_territories"
    background-color="white"
    :no-data-text="__('Territory not found')"
    hide-details
    :rules="[v => !!v || __('Territory is required')]"
  ></v-autocomplete>
                </v-col>
                <v-col cols="6">
                <v-checkbox v-model="is_group" label="Is Group"></v-checkbox>
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
      territoryDialog: false,
      territory_name:'',
      parent_territory:'',
      is_group:'',
      parent_territories:[],
      pos_profile:''
    }),
    watch: {
    },
    methods: {
      getParentTerritories(){
      const vm = this;
      frappe.db
        .get_list('Territory', {
          fields: ['name'],
          filters: {'is_group':1},
          page_length: 1000,
          limit:999999,
        })
        .then((data) => {
          if (data.length > 0) {
            data.forEach((el) => {
              vm.parent_territories.push(el.name);
            });
          }
        });
      },
      submit_dialog() {
        if (this.territory_name) {
          const vm = this;
          const args = {
            territory_name:this.territory_name,
            parent_territory:this.parent_territory,
            is_group:this.is_group?1:0
          };
          frappe.call({
            method: 'posawesome.posawesome.api.posapp.create_territory',
            args: args,
            callback: (r) => {
              if (!r.exc && r.message.name) {
                evntBus.$emit('show_mesage', {
                  text: __('Territory created successfully.'),
                  color: 'success',
                });
                args.name = r.message.name;
                vm.getParentTerritories();
                frappe.utils.play_sound('submit');
                evntBus.$emit('add_territory_to_list', {'name':r.message.name});
                evntBus.$emit('set_territory', r.message.name);
                vm.territory_name = '';
                vm.parent_territory = '';
                vm.is_group = '';
                vm.territoryDialog = false;
              }
              else{
                vm.territoryDialog = true;
              }
            },
          });
        }
      },
    },
    created: function () {
      this.getParentTerritories()
      evntBus.$on('open_new_territory', () => {
        this.territoryDialog = true;        
      });
      evntBus.$on('register_pos_profile', (data) => {
        this.pos_profile = data.pos_profile;
      });
    },
  };
  </script>
  