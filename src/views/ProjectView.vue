<template>
  <v-container
    fill-height
    fluid
    grid-list-xl>
    <v-layout
      justify-center
      wrap
    >
      <v-flex
        xs12
        md12
      >
        <material-card
          color="warning"
          :title="`${currentProjectData.projectName} - ${currentProjectData.projectNumber}`"
          :text="`Starts on ${currentProjectData.startDate} and ends on ${currentProjectData.endDate}`"
        >
    <v-layout
      justify-center
      wrap
    >
          <v-flex
            xs12
            md12
          >
                <material-notification
                  class="mb-3"
                  color="#eeeeee"
                  v-for="flow in scopedFlows"
                  v-bind:key="flow.id"
                >
                  <v-layout wrap>
                    <v-flex xs12 sm12 lg3>
                      <material-stats-card
                        color="success"
                        icon="mdi-chart-bubble"
                        :title="`${flow.flowCountry}`"
                        :value="`${flow.flowName}`"
                        sub-icon="mdi-calendar"
                        :sub-text="`${flow.startDate} to ${flow.endDate}`"
                      />
                    </v-flex>
                    <v-flex xs12 sm12 lg9>
                      <material-card
                      color="white"
                      class="flow__card"
                      >
                      <v-layout wrap>
                        <v-flex xs12 sm12 lg6>
                    <p class="card-title font-weight-light">accepted by 10/15 students</p>
                    <p class="card-title font-weight-light">accepted by 1/1 hosting companies</p>
                    <p class="card-title font-weight-light">accepted by 2/2 teachers</p>
                        </v-flex>
                        <v-flex xs12 sm12 lg2>
                          <v-tooltip
                            top
                            content-class="top">
                            <v-btn
                              slot="activator"
                              class="v-btn--simple flow__tooltip"
                              color="success"
                              icon
                              @click="studentDialog = true"
                            >
                              <v-icon color="primary" size="40">mdi-account-multiple-check</v-icon>
                            </v-btn>
                            <span>View Students</span>
                          </v-tooltip>
                        </v-flex>
                        <v-flex xs12 sm12 lg2>
                          <v-tooltip
                            top
                            content-class="top">
                            <v-btn
                              slot="activator"
                              class="v-btn--simple flow__tooltip"
                              color="success"
                              icon
                            >
                              <v-icon color="primary" size="40">mdi-account-edit</v-icon>
                            </v-btn>
                            <span>View Teachers</span>
                          </v-tooltip>
                        </v-flex>
                        <v-flex xs12 sm12 lg2>
                          <v-tooltip
                            top
                            content-class="top">
                            <v-btn
                              slot="activator"
                              class="v-btn--simple flow__tooltip"
                              color="success"
                              icon
                            >
                              <v-icon color="primary" size="40">mdi-account-group</v-icon>
                            </v-btn>
                            <span>View Hosting Companies</span>
                          </v-tooltip>
                        </v-flex>
                      </v-layout>
                      </material-card>
                    </v-flex>
                  </v-layout> 
                </material-notification>
          </v-flex>
    </v-layout>
        </material-card>
      </v-flex>
      <!-- <v-flex
        xs12
        md5
      >
        <material-card
          color="warning"
          :title="`${currentProjectData.projectName}`"
          text="Project details"
          class="mt-6"
        >
                  <p class="card-title font-weight-light"><v-icon small>mdi-calendar</v-icon> Project's start date: {{currentProjectData.startDate}}</p>
                  <p class="card-title font-weight-light"><v-icon small>mdi-calendar</v-icon> Project's end date: {{currentProjectData.endDate}}</p>
                  <p class="card-title font-weight-light"><v-icon small color="warning">mdi-hexagon-multiple</v-icon> Project number: {{currentProjectData.projectNumber}}</p>
        </material-card>
      </v-flex> -->
    </v-layout>

      <v-dialog v-model="studentDialog" scrollable max-width="1000">
        <v-card>
                  <material-card
                    color="green"
                    title="Student List"
                    text="This is a list of your added students"
                  >
                    <v-data-table
                      :headers="headers"
                      :items="scopedStudents"
                      hide-actions
                    >
                      <template
                        slot="headerCell"
                        slot-scope="{ header }"
                      >
                        <span
                          class="subheading font-weight-light text-success text--darken-3"
                          v-text="header.text"
                        />
                      </template>
                      <template
                        slot="items"
                        slot-scope="{ item }"
                      >
                        <td>{{ item.firstName }}</td>
                        <td>{{ item.lastName }}</td>
                        <td>{{ item.email }}</td>
                        <td>{{ item.flow }}</td>
                        <td>{{ item.acceptedFlow }}</td>
                        <td><v-icon color="success">mdi-pencil</v-icon></td>
                      </template>
                    </v-data-table>
                  </material-card>
          <v-divider></v-divider>
          <v-card-actions>
            <v-btn color="success" text @click="studentDialog = false">Close</v-btn>
          </v-card-actions>
        </v-card>
      </v-dialog>

      <v-dialog v-model="teacherDialog" scrollable max-width="1000">
        <v-card>
                  <material-card
                    color="green"
                    title="Teacher List"
                    text="This is a list of your added teachers"
                  >
                    <v-data-table
                      :headers="headers"
                      :items="teachers"
                      hide-actions
                    >
                      <template
                        slot="headerCell"
                        slot-scope="{ header }"
                      >
                        <span
                          class="subheading font-weight-light text-success text--darken-3"
                          v-text="header.text"
                        />
                      </template>
                      <template
                        slot="items"
                        slot-scope="{ item }"
                      >
                        <td>{{ item.name }}</td>
                        <td>{{ item.school }}</td>
                        <td>{{ item.email }}</td>
                        <td>{{ item.flow }}</td>
                        <td><v-icon color="success">mdi-pencil</v-icon></td>
                      </template>
                    </v-data-table>
                  </material-card>
          <v-divider></v-divider>
          <v-card-actions>
            <v-btn color="success" text @click="teacherDialog = false">Close</v-btn>
          </v-card-actions>
        </v-card>
      </v-dialog>

      <v-dialog v-model="companyDialog" scrollable max-width="1000">
        <v-card>
                  <material-card
                    color="green"
                    title="Hosting Companies"
                    text="This is a list of your added companies"
                  >
                    <v-data-table
                      :headers="headers"
                      :items="companies"
                      hide-actions
                    >
                      <template
                        slot="headerCell"
                        slot-scope="{ header }"
                      >
                        <span
                          class="subheading font-weight-light text-success text--darken-3"
                          v-text="header.text"
                        />
                      </template>
                      <template
                        slot="items"
                        slot-scope="{ item }"
                      >
                        <td>{{ item.name }}</td>
                        <td>{{ item.school }}</td>
                        <td>{{ item.email }}</td>
                        <td>{{ item.flow }}</td>
                        <td><v-icon color="success">mdi-pencil</v-icon></td>
                      </template>
                    </v-data-table>
                  </material-card>
          <v-divider></v-divider>
          <v-card-actions>
            <v-btn color="success" text @click="companyDialog = false">Close</v-btn>
          </v-card-actions>
        </v-card>
      </v-dialog>

  </v-container>
</template>

<script>

import { db } from '@/main'
import { mapGetters } from 'vuex';
import store from '@/main';

export default {
  data() {
    return {
      headers: [
        {
          sortable: false,
          text: 'First Name',
          value: 'name'
        },
        {
          sortable: false,
          text: 'Last Name',
          value: 'school'
        },
        {
          sortable: false,
          text: 'Email',
          value: 'email'
        },
        {
          sortable: false,
          text: 'Assigned Flow',
          value: 'flow'
        },
        {
          sortable: false,
          text: 'Accepted Flow?',
          value: 'flow'
        }
      ],
      students: [
        {
        name: "pdsa",
        school: "sfasfasf",
        email: "asd@asda.com",
        flow: "fdkjgbsdfgjkbs"
        },
        {
        name: "pdsa",
        school: "sfasfasf",
        email: "asd@asda.com",
        flow: "fdkjgbsdfgjkbs"
        }
      ],
      teachers: [{
        name: "pdsa",
        school: "sfasfasf",
        email: "asd@asda.com",
        flow: "fdkjgbsdfgjkbs"
      }],
      companies: [{
        name: "pdsa",
        school: "sfasfasf",
        email: "asd@asda.com",
        flow: "fdkjgbsdfgjkbs"
      }],
      studentDialog: false,
      teacherDialog: false,
      companyDialog: false,
      notifications: false
    }
  },
  computed: {
    ...mapGetters([
      "getFlows",
      "getProjects",
      "getStudents"
    ]),
    currentProjectData() {
      let b = this.getProjects.find(o => o.id === this.$route.params.projectId);
      return b;
    },
    scopedFlows() {
      const data = this.getFlows
      const flows = [];
      for(let i=0; i < data.length; i++) {
        if (data[i].parentProject === this.$route.params.projectId) {
          flows.push(data[i])
        }
      }
      return flows;
    },
    scopedStudents() {
      let students = []
      this.getStudents.forEach(a => {
        this.scopedFlows.forEach(f => {
          if(f.flowName == a.flow) {
            students.push(a)
          }
        })
      })
      // db.collection('users').where("student", "==", true).get().then((snapshot) => {
      //   snapshot.forEach(doc => {
      //     const std = doc.data();
      //     this.scopedFlows.forEach(f => {
      //       if(f.flowName === std.flow) {
      //         students.push(std)
      //       } else {
      //         console.log('error in 2nd if')
      //       }
      //     })
      //   })
      // })
      return students;
    }
  },
  methods: {
  }
}
</script>

<style>
.smaller_box > .v-card--material-stats.v-card .v-offset .v-card {
  max-width: 60px !important;
  max-height: 60px !important;
}
.flow__card {
  margin-top: 25px !important;
}
.flow__tooltip {
  margin-top: 20px !important;
}
</style>
