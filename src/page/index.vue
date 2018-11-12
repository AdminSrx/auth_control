<template>
  <v-app id="inspire">
    <v-navigation-drawer :clipped="$vuetify.breakpoint.lgAndUp" v-model="drawer" fixed app>
      <v-list dense>
        <template v-for="item in items">
                                        <v-layout
                                          v-if="item.heading"
                                          :key="item.heading"
                                          row
                                          align-center
                                        >
                                          <v-flex xs6>
                                            <v-subheader v-if="item.heading">
                                              {{ item.heading }}
                                            </v-subheader>
                                          </v-flex>
                                          <v-flex xs6 class="text-xs-center">
                                            <a href="#!" class="body-2 black--text">EDIT</a>
                                          </v-flex>
                                        </v-layout>
                                        <v-list-tile v-else :key="item.text" @click="">
                                          <v-list-tile-action>
                                            <v-icon>{{ item.icon }}</v-icon>
                                          </v-list-tile-action>
                                          <v-list-tile-content>
                                            <v-list-tile-title>
                                              {{ item.text }}
                                            </v-list-tile-title>
                                          </v-list-tile-content>
                                        </v-list-tile>
</template>
      </v-list>
    </v-navigation-drawer>
    <v-toolbar
      :clipped-left="$vuetify.breakpoint.lgAndUp"
      color="blue darken-3"
      dark
      app
      fixed
    >
      <v-toolbar-title style="width: 300px" class="ml-0 pl-3">
        <v-toolbar-side-icon @click.stop="drawer = !drawer" class="material-icons">reorder</v-toolbar-side-icon>
        <span class="hidden-sm-and-down">激活界面</span>
      </v-toolbar-title>
    
      <v-spacer></v-spacer>
             <div class="text-xs-center">
    <v-dialog
      v-model="dialogs"
      width="500"
    >
          <i class="material-icons" slot="activator">
account_circle
</i>  
    
      <v-card>
        <v-card-title
          class="headline dark lighten-2"
          primary-title
        >
    登录
        </v-card-title>

        <v-card-text>
       <v-form>
                  <v-text-field prepend-icon="person" name="login" label="Login" type="text"></v-text-field>
                  <v-text-field prepend-icon="lock" name="password" label="Password" id="password" type="password"></v-text-field>
                    </v-form>
        </v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn color="primary">登录</v-btn>
        </v-card-actions>
      </v-card>
    </v-content>
    </v-dialog>
  </div >
     
    </v-toolbar>
    <v-content > 
      <v-container fluid fill-height>
        <v-layout justify-center align-center>        
        <!-- 内容 -->
        <div style="width:100%">
    <v-toolbar flat color="white">
      <v-toolbar-title>表格</v-toolbar-title>
       <v-spacer></v-spacer>
      <v-divider
        class="mx-2"
        inset
        vertical
      ></v-divider>
      <v-spacer> <v-text-field
        v-model="search"
        append-icon="search"
        label="搜索"
        single-line
        hide-details
      ></v-text-field></v-spacer>
      <v-dialog v-model="dialog" max-width="500px">
        
        <v-btn slot="activator" color="primary" dark class="mb-2">新建</v-btn>
        <v-card>
          <v-card-title>
            <span class="headline">{{ formTitle }}</span>
          </v-card-title>
        <!-- 新建表 -->
          <v-card-text>
            <v-container grid-list-md>
              <v-layout wrap>
              
                <v-flex xs12 sm6 md6>
                  <v-text-field v-model="editedItem.fat" label="username"></v-text-field>
                </v-flex>
                <v-flex xs12 sm6 md6>
                  <v-text-field v-model="editedItem.carbs" label="expiredAt"></v-text-field>
                </v-flex>
               
                <v-flex xs12 sm6 md6>
                  <v-text-field v-model="newPayload" label="applicationPayload
"></v-text-field>
                </v-flex>
                 <v-flex  xs12 sm6 md6>
                     <v-btn @click.native="show">+</v-btn>
                  </v-flex>
           
             <v-card class="elevation-2" xs12 sm12 md12>
               
       <v-card-text>
         {{_applicationPayload}}
        </v-card-text>
        </v-card>
              </v-layout>
            </v-container>
          </v-card-text>

          <v-card-actions>
            <v-spacer></v-spacer>        
            <v-btn color="blue darken-1" flat @click.native="save">添加</v-btn>
             <v-btn color="blue darken-1" flat @click.native="close">关闭</v-btn>
          </v-card-actions>
        </v-card>
      </v-dialog>
    </v-toolbar>
    <v-data-table
      :headers="headers"
      :items="desserts"
      hide-actions
      class="elevation-1"
    >
<template slot="items" slot-scope="props">
  <td class="text-xs-left">
    {{ props.item.code }}</td>
  <td class="text-xs-right">
    <v-edit-dialog :return-value.sync="props.item.username" large lazy persistent @save="saveinfo" @cancel="cancel" @open="open" @close="close">
      <div>{{ props.item.username }}</div>
      <div slot="input" class="mt-3 title">更改</div>
      <v-text-field slot="input" v-model="props.item.username" :rules="[max25chars]" label="Edit" single-line counter autofocus></v-text-field>
    </v-edit-dialog>
  </td>
  <td class="text-xs-left">{{ props.item.expired }}</td>
  <td class="text-xs-left">{{ props.item.online }}</td>
  <td class="text-xs-left">{{ props.item.LastUpdate }}</td>
  <!-- <td class="text-xs-left">{{props.item.applicationPayload.toString()}}</td> -->
  <td class="text-xs-right">
    <v-edit-dialog :return-value.sync="props.item.applicationPayload.toString()" large lazy @save="saveinfo" @cancel="cancel" @open="open" @close="close">
      <div>{{ props.item.applicationPayload.toString()}}</div>
      <div slot="input" class="mt-3 title">更改</div>
      <!-- <v-card-text slot="input" style="">{{props.item.applicationPayload.toString()}}</v-card-text> -->
       <v-text-field slot="input" v-model="props.item.applicationPayload.toString()" :rules="[max25chars]" label="Edit" single-line counter autofocus></v-text-field>
    </v-edit-dialog>
  </td>
  <!-- <td class="text-xs-left" @click="editItem(props.item)">
              <v-dialog v-model="dialoga" max-width="800px">
                <v-btn slot="activator"> 详情</v-btn>
                <v-card>
                  <v-card-text>
                    <v-container grid-list-md>
                      <v-layout wrap>
                        <v-flex xs12 sm6 md3>
                          <v-text-field v-model="editedItem.fat" label="username"></v-text-field>
                        </v-flex>
                        
                      </v-layout>
                    </v-container>
                  </v-card-text>
                  <v-card-actions>
                    <v-spacer></v-spacer>
                    <v-btn color="blue darken-1" flat @click.native="save">添加</v-btn>
                    <v-btn color="blue darken-1" flat @click.native="close">关闭</v-btn>
                  </v-card-actions>
                </v-card>
              </v-dialog>
            </td> -->
  <td class="justify-center layout px-0">
    <!-- <v-icon small class="mr-2" @click="editItem(props.item)">
                        edit
                      </v-icon> -->
    <v-icon small @click="deleteItem(props.item)">
      delete
    </v-icon>
  </td>
</template>

<template slot="no-data">
  <v-btn color="primary" @click="initialize">
    Reset</v-btn>
</template>
    </v-data-table>
    </div>
        </v-layout>
      </v-container>
     </v-content> 
   
  </v-app>
</template>

<script>
  export default {
    data: () => ({
      newPayload: '1',
      _applicationPayload:[],
      dialogs: false,
      dialog: false,
      drawer: null,
      items: [{
          icon: 'contacts',
          text: '激活列表A'
        },
        {
          icon: 'contacts',
          text: '激活列表B'
        }
      ],
      headers: [{
          text: 'code',
          align: 'left',
          sortable: false,
          value: 'code'
        },
        {
          text: 'username',
          value: 'username'
        },
        {
          text: 'expired',
          value: 'expired'
        },
        {
          text: 'online',
          value: 'online'
        },
        {
          text: 'lastUpdate',
          value: 'lastUpdate'
        }, {
          text: 'applicationPayload',
          value: 'applicationPayload'
        },
        {
          text: 'Actions',
          value: 'id',
          sortable: false
        }
      ],
      desserts: [],
      editedIndex: -1,
      editedItem: {
        id: 0,
        code: 0,
        username: 0,
        identifier: 0,
        isInited: 0,
        lastUpdate: null,
        createAt: '',
        statusPayload: [],
        applicationPayload: [],
        expiredAt: '',
        expired: '',
        online: ''
      },
      defaultItem: {
        id: 0,
        code: 0,
        username: 0,
        identifier: 0,
        isInited: 0,
        lastUpdate: null,
        createAt: '',
        statusPayload: [],
        applicationPayload: [],
        expiredAt: '',
        expired: '',
        online: ''
      }
    }),
    props: {
      source: String
    },
    computed: {
      formTitle() {
        return this.editedIndex === -1 ? 'New Item' : 'Edit Item'
      }
    },
    watch: {
      dialog(val) {
        val || this.close()
      }
    },
    created() {
      this.initialize()
    },
    methods: {
      initialize() {
        this.desserts = [{
            id: 1,
            code: "rgKm-Ebx-YKIT-Ny-tHG0",
            username: '1.神奇制造',
            identifier: null,
            isInited: false,
            lastUpdate: null,
            createAt: '2018-11-07T08:39:53.169Z',
            statusPayload: ["3", "4"],
            applicationPayload: ["1", "2", "1", "2", "1", "2", "1", "2", "1", "2", "1", "2"],
            expiredAt: 1541582539347,
            expired: true,
            online: false
          },
          {
            id: 2,
            code: "rgKm-Ebx-YKIT-Ny-tHG0",
            username: '2.神奇制造',
            identifier: null,
            isInited: false,
            lastUpdate: null,
            createAt: '2018-11-07T08:39:53.169Z',
            statusPayload: ["3", "4"],
            applicationPayload: ["1", "2"],
            expiredAt: 1541582539347,
            expired: true,
            online: false
          },
          {
            id: 3,
            code: "rgKm-Ebx-YKIT-Ny-tHG0",
            username: '3.神奇制造',
            identifier: null,
            isInited: false,
            lastUpdate: null,
            createAt: '2018-11-07T08:39:53.169Z',
            statusPayload: ["3", "4"],
            applicationPayload: ["1", "2"],
            expiredAt: 1541582539347,
            expired: true,
            online: false
          },
          {
            id: 4,
            code: "rgKm-Ebx-YKIT-Ny-tHG0",
            username: '4.神奇制造',
            identifier: null,
            isInited: false,
            lastUpdate: null,
            createAt: '2018-11-07T08:39:53.169Z',
            statusPayload: ["3", "4"],
            applicationPayload: ["1", "2"],
            expiredAt: 1541582539347,
            expired: true,
            online: false
          },
          {
            id: 4,
            code: "rgKm-Ebx-YKIT-Ny-tHG0",
            username: '4.神奇制造',
            identifier: null,
            isInited: false,
            lastUpdate: null,
            createAt: '2018-11-07T08:39:53.169Z',
            statusPayload: ["3", "4"],
            applicationPayload: ["1", "2"],
            expiredAt: 1541582539347,
            expired: true,
            online: false
          },
          {
            id: 4,
            code: "rgKm-Ebx-YKIT-Ny-tHG0",
            username: '4.神奇制造',
            identifier: null,
            isInited: false,
            lastUpdate: null,
            createAt: '2018-11-07T08:39:53.169Z',
            statusPayload: ["3", "4"],
            applicationPayload: ["1", "2"],
            expiredAt: 1541582539347,
            expired: true,
            online: false
          },
          {
            id: 4,
            code: "rgKm-Ebx-YKIT-Ny-tHG0",
            username: '4.神奇制造',
            identifier: null,
            isInited: false,
            lastUpdate: null,
            createAt: '2018-11-07T08:39:53.169Z',
            statusPayload: ["3", "4"],
            applicationPayload: ["1", "2"],
            expiredAt: 1541582539347,
            expired: true,
            online: false
          },
          {
            id: 4,
            code: "rgKm-Ebx-YKIT-Ny-tHG0",
            username: '4.神奇制造',
            identifier: null,
            isInited: false,
            lastUpdate: null,
            createAt: '2018-11-07T08:39:53.169Z',
            statusPayload: ["3", "4"],
            applicationPayload: ["1", "2"],
            expiredAt: 1541582539347,
            expired: true,
            online: false
          },
          {
            id: 4,
            code: "rgKm-Ebx-YKIT-Ny-tHG0",
            username: '4.神奇制造',
            identifier: null,
            isInited: false,
            lastUpdate: null,
            createAt: '2018-11-07T08:39:53.169Z',
            statusPayload: ["3", "4"],
            applicationPayload: ["1", "2"],
            expiredAt: 1541582539347,
            expired: true,
            online: false
          },
          {
            id: 4,
            code: "rgKm-Ebx-YKIT-Ny-tHG0",
            username: '4.神奇制造',
            identifier: null,
            isInited: false,
            lastUpdate: null,
            createAt: '2018-11-07T08:39:53.169Z',
            statusPayload: ["3", "4"],
            applicationPayload: ["1", "2"],
            expiredAt: 1541582539347,
            expired: true,
            online: false
          },
          {
            id: 4,
            code: "rgKm-Ebx-YKIT-Ny-tHG0",
            username: '4.神奇制造',
            identifier: null,
            isInited: false,
            lastUpdate: null,
            createAt: '2018-11-07T08:39:53.169Z',
            statusPayload: ["3", "4"],
            applicationPayload: ["1", "2"],
            expiredAt: 1541582539347,
            expired: true,
            online: false
          },
          {
            id: 4,
            code: "rgKm-Ebx-YKIT-Ny-tHG0",
            username: '4.神奇制造',
            identifier: null,
            isInited: false,
            lastUpdate: null,
            createAt: '2018-11-07T08:39:53.169Z',
            statusPayload: ["3", "4"],
            applicationPayload: ["1", "2"],
            expiredAt: 1541582539347,
            expired: true,
            online: false
          },
          {
            id: 4,
            code: "rgKm-Ebx-YKIT-Ny-tHG0",
            username: '4.神奇制造',
            identifier: null,
            isInited: false,
            lastUpdate: null,
            createAt: '2018-11-07T08:39:53.169Z',
            statusPayload: ["3", "4"],
            applicationPayload: ["1", "2"],
            expiredAt: 1541582539347,
            expired: true,
            online: false
          },
          {
            id: 4,
            code: "rgKm-Ebx-YKIT-Ny-tHG0",
            username: '4.神奇制造',
            identifier: null,
            isInited: false,
            lastUpdate: null,
            createAt: '2018-11-07T08:39:53.169Z',
            statusPayload: ["3", "4"],
            applicationPayload: ["1", "2"],
            expiredAt: 1541582539347,
            expired: true,
            online: false
          },
          {
            id: 4,
            code: "rgKm-Ebx-YKIT-Ny-tHG0",
            username: '4.神奇制造',
            identifier: null,
            isInited: false,
            lastUpdate: null,
            createAt: '2018-11-07T08:39:53.169Z',
            statusPayload: ["3", "4"],
            applicationPayload: ["1", "2"],
            expiredAt: 1541582539347,
            expired: true,
            online: false
          },
          {
            id: 4,
            code: "rgKm-Ebx-YKIT-Ny-tHG0",
            username: '4.神奇制造',
            identifier: null,
            isInited: false,
            lastUpdate: null,
            createAt: '2018-11-07T08:39:53.169Z',
            statusPayload: ["3", "4"],
            applicationPayload: ["1", "2"],
            expiredAt: 1541582539347,
            expired: true,
            online: false
          }
        ]
      },
      editItem(item) {
        this.editedIndex = this.desserts.indexOf(item)
        this.editedItem = Object.assign({}, item)
        this.dialog = true
      },
      deleteItem(item) {
        const index = this.desserts.indexOf(item)
        confirm('Are you sure you want to delete this item?') && this.desserts.splice(index, 1)
      },
      close() {
        this.dialog = false
        setTimeout(() => {
          this.editedItem = Object.assign({}, this.defaultItem)
          this.editedIndex = -1
        }, 300)
      },
      save() {
        if (this.editedIndex > -1) {
          Object.assign(this.desserts[this.editedIndex], this.editedItem)
        } else {
          this.desserts.push(this.editedItem)
        }
        this.close()
      },
      saveinfo() {
        if (this.editedIndex > -1) {
          Object.assign(this.desserts[this.editedIndex], this.editedItem)
        }
        this.close()
      }
    }
  }
</script>
<style scoped>
  /* .v-content{    padding-top: 0px !important;}  */
  td .material-icons {
    line-height: 3
  }
</style>
