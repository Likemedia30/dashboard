<template>
  <v-container grid-list-xl fluid>
    <v-flex lg3 v-for="(item,index) in users" :key=" 'basic' + index">
      <v-card>
        <name-card v-bind="item">
          </name-card>
          <v-card-actions>
            <v-switch :label="s1 ? 'Online': 'Offline'" v-model="s1"></v-switch>
          </v-card-actions>
      </v-card>
    </v-flex>
    <v-layout row wrap>
      <v-flex lg6 sm12 xs12>
        <message-list></message-list>
      </v-flex>
      <v-flex lg6 sm12 xs12>
        <notification-list></notification-list>
      </v-flex>
    </v-layout>
    <v-layout row wrap>
        <v-flex sm12>
          <h3 class="text-sm-center font-weight-medium">Calendar</h3>
        </v-flex>
        <v-flex lg3>
          <v-widget title="Basic Usage">
            <div slot="widget-content">
              <div class="flexbox-centering">
                <v-date-picker color="green lighten-1" v-model="picker"></v-date-picker>
              </div>
            </div>
          </v-widget>
        </v-flex>

        <v-flex lg9 sm12>
          <v-card>
            <v-card-text class="pa-0">
              <v-data-table
                :headers="complex.headers"
                :search="search"
                :items="complex.items"
                :rows-per-page-items="[10,25,50,{text:'All','value':-1}]"
                class="elevation-1"
                item-key="name"
                v-model="complex.selected"
              >
                <template slot="items" slot-scope="props">
                  <td>
                    {{ props.item.start }}
                  </td>
                  <td>
                    {{ props.item.end }}
                  </td>
                  <td>
                    {{ props.item.name }}
                  </td>
                  <td>
                    {{ props.item.email }}
                  </td>
                  <td>
                    <v-btn depressed outline icon fab dark color="primary" small>
                      <v-icon>send</v-icon>
                    </v-btn>
                  </td>
                  <td>
                    <v-btn  outline  fab dark color="primary" small>
                      <v-icon>adjust</v-icon>
                    </v-btn>
                  </td>
                </template>
              </v-data-table>
            </v-card-text>
          </v-card>
        </v-flex>
                <v-flex sm12>
          <h3 class="text-sm-center font-weight-medium">Clients</h3>
        </v-flex>
        <v-flex lg12>
                    <v-card>
            <v-toolbar card color="white">
              <v-text-field
                flat
                solo
                prepend-icon="search"
                placeholder="Type something"
                v-model="search"
                hide-details
                class="hidden-sm-and-down"
              ></v-text-field>
              <v-btn icon>
                <v-icon>filter_list</v-icon>
              </v-btn>
            </v-toolbar>
            <v-divider></v-divider>
            <v-card-text class="pa-0">
              <v-data-table
                :headers="clients.headers"
                :search="search"
                :items="clients.items"
                :rows-per-page-items="[10,25,50,{text:'All','value':-1}]"
                class="elevation-1"
                item-key="name"
                select-all
                v-model="complex.selected"
              >
                <template slot="items" slot-scope="props">
                  <td>
                    <v-checkbox
                      primary
                      hide-details
                      v-model="props.selected"
                    ></v-checkbox>
                  </td>
                  <td>
                    <v-avatar size="32">
                      <img :src="props.item.avatar" alt="">
                    </v-avatar>
                  </td>
                  <td>
                    <span>{{ props.item.name }}</span>
                    <span>{{ props.item.email }}</span>
                  </td>
                  <td>
                    <span>Functia: {{ props.item.func }}</span>
                    <span>Rol: {{ props.item.rol }}</span>
                  </td>
                  <td>
                    <v-btn depressed outline icon fab dark color="primary" small>
                      <v-icon>edit</v-icon>
                    </v-btn>
                  </td>
                  <td>
                    <v-btn  outline  fab dark color="primary" small>
                      <v-icon>link</v-icon>
                    </v-btn>
                  </td>
                  <td>
                    <v-btn  outline  fab dark color="primary" small>
                      <v-icon>event</v-icon>
                    </v-btn>
                  </td>
                  <td>
                    <v-btn  outline  fab dark color="primary" small>
                      <v-icon>phone</v-icon>
                    </v-btn>
                  </td>
                  <td>
                    <v-btn  outline  fab dark color="primary" small>
                      <v-icon>mail</v-icon>
                    </v-btn>
                  </td>
                  <td>
                    <v-btn depressed outline icon fab dark color="pink" small>
                      <v-icon>delete</v-icon>
                    </v-btn>
                  </td>
                </template>
              </v-data-table>
            </v-card-text>
          </v-card>
        </v-flex>
    </v-layout>    
  </v-container>
</template>

<script>
  import NameCard from '@/components/widgets/card/NameCard';
  import ProfileCard from '@/components/widgets/card/ProfileCard';
  import MessageList from '@/components/widgets/list/MessageList';
  import NotificationList from '@/components/widgets/list/NotificationList';
  import {Items as Users} from '@/api/user';
  import VWidget from '@/components/VWidget';

  export default {
    layout: "dashboard",
    components: {
      NameCard,
      ProfileCard,
      MessageList,
      NotificationList,
      VWidget
    },
    data() {
      return {
        picker: null,
        picker2: null,
        //
        arrayEvents: null,
        date1: null,
        date2: null,
        //
        date: null,
        menu: false,
        modal: false,
        s1: false,
        users: [
          {
            jobTitle: 'CRManager',
            name: 'Jessie J',
            color: 'blue',
            dark: true,
            avatar: {
              src: 'https://randomuser.me/api/portraits/women/1.jpg',
              size: '36'
            },
          },
        ],
        complex: {
            headers: [
              {
                text: 'Start',
                value: 'start'
              },
              {
                text: 'End',
                value: 'end'
              },
              {
                text: 'Name',
                value: 'name'
              },
              {
                text: 'Email',
                value: 'email'
              },
              {
                text: 'Send Email',
                value: 'send'
              },
              {
                text: 'Join Meeting',
                value: 'join'
              }
            ],
            items: Users
          },
        clients: {
            selected: [],
            headers: [
              {
                text: 'Avatar',
                value: 'avatar'
              },
              {
                text: 'Info',
                value: 'info'
              },
              {
                text: 'Role',
                value: 'role'
              },
              {
                text: 'Edit',
                value: 'edit'
              },
              {
                text: 'Copy Link',
                value: 'link'
              },
              {
                text: 'Event',
                value: 'event'
              },
              {
                text: 'Call',
                value: 'call'
              },
              {
                text: 'Message',
                value: 'message'
              },
              {
                text: 'Delete',
                value: 'delete'
              }
            ],
            items: Users
          },
          search: '',
          basic: {
            headers: [
              {
                text: 'Dessert (100g serving)',
                align: 'left',
                sortable: false,
                value: 'name'
              },
              {text: 'Calories', value: 'calories'},
              {text: 'Fat (g)', value: 'fat'},
              {text: 'Carbs (g)', value: 'carbs'},
              {text: 'Protein (g)', value: 'protein'},
              {text: 'Iron (%)', value: 'iron'}
            ],
            items: [
              {
                value: false,
                name: 'Frozen Yogurt',
                calories: 159,
                fat: 6.0,
                carbs: 24,
                protein: 4.0,
                iron: '1%'
              },
              {
                value: false,
                name: 'Ice cream sandwich',
                calories: 237,
                fat: 9.0,
                carbs: 37,
                protein: 4.3,
                iron: '1%'
              },
              {
                value: false,
                name: 'Eclair',
                calories: 262,
                fat: 16.0,
                carbs: 23,
                protein: 6.0,
                iron: '7%'
              },
              {
                value: false,
                name: 'Cupcake',
                calories: 305,
                fat: 3.7,
                carbs: 67,
                protein: 4.3,
                iron: '8%'
              },
              {
                value: false,
                name: 'Gingerbread',
                calories: 356,
                fat: 16.0,
                carbs: 49,
                protein: 3.9,
                iron: '16%'
              },
              {
                value: false,
                name: 'Jelly bean',
                calories: 375,
                fat: 0.0,
                carbs: 94,
                protein: 0.0,
                iron: '0%'
              },
              {
                value: false,
                name: 'Lollipop',
                calories: 392,
                fat: 0.2,
                carbs: 98,
                protein: 0,
                iron: '2%'
              },
              {
                value: false,
                name: 'Honeycomb',
                calories: 408,
                fat: 3.2,
                carbs: 87,
                protein: 6.5,
                iron: '45%'
              },
              {
                value: false,
                name: 'Donut',
                calories: 452,
                fat: 25.0,
                carbs: 51,
                protein: 4.9,
                iron: '22%'
              },
              {
                value: false,
                name: 'KitKat',
                calories: 518,
                fat: 26.0,
                carbs: 65,
                protein: 7,
                iron: '6%'
              }
            ]
          }
      };
    },
    mounted() {
      this.arrayEvents = [...Array(6)].map(() => {
        const day = Math.floor(Math.random() * 30);
        const d = new Date();
        d.setDate(day);
        return d.toISOString().substr(0, 10);
      });
    },
    computed: {
      posts() {
        return getPost();
      }
    },
    methods: {
      functionEvents(date) {
        const [, , day] = date.split('-');
        return parseInt(day, 10) % 3 === 0;
      },
      handleClick: (e) => {
        console.log(e);
      }
    }
  };
</script>

<style scoped>
  .subheading {
    font-weight: 500;
    font-size: 18px !important;
  }
  .caption {
    font-weight: 400;
    font-size: 16px !important;
  }
</style>
