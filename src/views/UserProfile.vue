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
        md6
      >
        <material-card>
          <v-avatar
            slot="offset"
            class="mx-auto d-block"
            size="30"
          >
          </v-avatar>
          <v-card-text class="text-xs">
            <h6 class="category text-gray mb-2">Assigned Flow</h6>
            <h4 class="card-title">{{user.userDetails.data.flow}}</h4>
            <p class="">This flow is taking place in {{assignedFlow.flowCountry}} between {{assignedFlow.startDate}} and {{assignedFlow.endDate}}</p>
            <v-btn
              color="warning"
              class="font-weight-light"
              v-if="!userDetails.acceptedFlow"
              @click="doc = true"
            >Accept flow now</v-btn>
            <v-btn
              slot="activator"
              color="success"
              class="font-weight-light"
              to="/document"
              v-if="userDetails.acceptedFlow"
            >Flow Accepted on {{userDetails.acceptedFlow}} - click to view document</v-btn>
          </v-card-text>
        </material-card>
        <material-card>
          <v-card-text class="text-xs">
            <h6 class="category text-gray mb-3">personal competencies</h6>
              <material-notification
                  class="mb-3"
                  v-for="item in PersonalType"
                  v-bind:key="item.id"
                  :color="`${item.color}`"
              >
                <v-layout>
                  <v-flex>
                  <p>{{item.name}}</p> Currently you are at "{{item.level}}" level. Earned an upgrade?
                  </v-flex>
                  <v-flex
                  md3
                  >
                    <v-btn
                      slot="activator"
                      class="v-btn--simple"
                      color="transparent"
                      @click="upgrade(item.name)"
                    >
                      <v-icon color="white">mdi-apple-keyboard-caps</v-icon>
                    </v-btn>
                  </v-flex>
                </v-layout>  
              </material-notification>
          </v-card-text>
        </material-card>
        <material-card>
          <v-card-text class="text-xs">
            <h6 class="category text-gray mb-3">vocational competencies</h6>
              <material-notification
                  class="mb-3"
                  v-for="item in VocationalType"
                  v-bind:key="item.id"
                  :color="`${item.color}`"
              >
                <v-layout>
                  <v-flex>
                  <p>{{item.name}}</p> Currently you are at "{{item.level}}" level. Earned an upgrade?
                  </v-flex>
                  <v-flex
                  md3
                  >
                    <v-btn
                      slot="activator"
                      class="v-btn--simple"
                      color="transparent"
                      @click="upgrade(item.name)"
                    >
                      <v-icon color="white">mdi-apple-keyboard-caps</v-icon>
                    </v-btn>
                  </v-flex>
                </v-layout>  
              </material-notification>
          </v-card-text>
        </material-card>
      </v-flex>
      <v-flex
        xs12
        md6
      >
        <material-card
          color="green"
          title="Personal Information"
          text="Below is the information submitted by the Admin. Use Notify Admin in case of errors."
        >
          <v-form>
            <v-container py-0>
              <v-layout wrap>
                <v-flex
                  xs12
                  md4
                >
                  <v-text-field
                    v-model="user.userDetails.data.firstName"
                    disabled
                    label="First Name"
                    class="purple-input"/>
                </v-flex>                
                <v-flex
                  xs12
                  md4
                >
                  <v-text-field
                    v-model="user.userDetails.data.lastName"
                    disabled
                    label="Last Name"
                    class="purple-input"/>
                </v-flex>
                <v-flex
                  xs12
                  md4
                >
                  <v-text-field
                    v-model="user.userDetails.data.gender"
                    disabled
                    label="Gender"
                    class="purple-input"/>
                </v-flex>
                <v-flex
                  xs12
                  md4
                >
                  <v-text-field
                    v-model="user.userDetails.data.email"
                    disabled
                    label="Email Address"
                    class="purple-input"/>
                </v-flex>
                <v-flex
                  xs12
                  md4
                >
                  <v-text-field
                    v-model="user.userDetails.data.phoneNumber"
                    disabled
                    label="Phone Number"
                    class="purple-input"/>
                </v-flex>
                <v-flex
                  xs12
                  md4
                >
                  <v-text-field
                    v-model="user.userDetails.data.dateOfBirth"
                    disabled
                    label="Date of Birth"
                    class="purple-input"/>
                </v-flex>
                <v-flex
                  xs12
                  md6
                >
                  <v-text-field
                    v-model="user.userDetails.data.sendingOrg"
                    disabled
                    class="purple-input"
                    label="Sending Organization"
                  />
                </v-flex>
                <v-flex
                  xs12
                  md6
                >
                  <v-text-field
                    v-model="user.userDetails.data.specialization"
                    disabled
                    class="purple-input"
                    label="Specialization"
                  />
                </v-flex>
                <v-flex
                  xs12
                  md12
                >
                  <v-text-field
                    v-model="user.userDetails.data.address"
                    disabled
                    class="purple-input"
                    label="Home Address"
                  />
                </v-flex>
                <v-flex
                  xs12
                  text-xs-center
                >
                <p>if the information you are seeing is not correct, please notify the Admin:</p>
                  <v-btn
                    class="mx-0 font-weight-light"
                    color="success"
                    @click="notifyAdmin = true"
                  >
                    Notify Admin
                  </v-btn>
                </v-flex>
              </v-layout>
            </v-container>
          </v-form>
        </material-card>
      </v-flex>
    </v-layout>
                  <v-dialog  class="accept_bk" v-model="doc" max-width="1200">
                    <v-flex
                      xs12
                      md12
                    >
                    <v-card height="500">
                      <material-card
                        color="success"
                        title="Accept this Flow"
                        text="You must review the information here before moving on"
                        center
                      >
                        <v-layout wrap>
                          <v-flex 
                            xs12
                            md7
                          >
                            <v-img
                              :src="video"
                              max-width="650"
                              contain
                            />
                          </v-flex>
                          <v-flex 
                            xs12
                            md5
                          >
                            <material-notification
                              class="mb-3"
                              color="info"
                              icon="mdi-alert-circle-outline"
                            >

                              <p>Please watch the video and acknowledge that ... so help me God, if you don't accept, i'm going to flip.</p>
                              <p>By the way, by clicking the 'accept' button, you will digitally sign the following documents:</p>
                            </material-notification>
                            <p><v-icon color="black">mdi-archive</v-icon>ERASMUS + LEARNING AGREEMENT FOR VET MOBILITY</p>
                            <p><v-icon color="black">mdi-archive</v-icon>To be decided</p>
                            <p><v-icon color="black">mdi-archive</v-icon>To be decided</p>
                            <p><v-icon color="black">mdi-archive</v-icon>To be decided</p>
                            <div class="text-xs-center">
                              <v-btn
                                class="mx-0 font-weight-light"
                                color="success"
                                bottom
                                @click="acceptFlow"
                              >
                                Accept the flow now
                              </v-btn>
                            </div>
                          </v-flex>
                        </v-layout>
                      </material-card>
                    </v-card>
                    </v-flex>
                    </v-dialog>
                                      <v-dialog  class="accept_bk" v-model="doc" max-width="1200">
                    <v-flex
                      xs12
                      md12
                    >
                    <v-card height="500">
                      <material-card
                        color="success"
                        title="Accept this Flow"
                        text="You must review the information here before moving on"
                        center
                      >
                        <v-layout wrap>
                          <v-flex 
                            xs12
                            md7
                          >
                            <v-img
                              :src="video"
                              max-width="650"
                              contain
                            />
                          </v-flex>
                          <v-flex 
                            xs12
                            md5
                          >
                            <material-notification
                              class="mb-3"
                              color="info"
                              icon="mdi-alert-circle-outline"
                            >

                              <p>Please watch the video and acknowledge that ... so help me God, if you don't accept, i'm going to flip.</p>
                              <p>By the way, by clicking the 'accept' button, you will digitally sign the following documents:</p>
                            </material-notification>
                            <p><v-icon color="black">mdi-archive</v-icon>ERASMUS + LEARNING AGREEMENT FOR VET MOBILITY</p>
                            <p><v-icon color="black">mdi-archive</v-icon>To be decided</p>
                            <p><v-icon color="black">mdi-archive</v-icon>To be decided</p>
                            <p><v-icon color="black">mdi-archive</v-icon>To be decided</p>
                            <div class="text-xs-center">
                              <v-btn
                                class="mx-0 font-weight-light"
                                color="success"
                                bottom
                                @click="acceptFlow"
                              >
                                Accept the flow now
                              </v-btn>
                            </div>
                          </v-flex>
                        </v-layout>
                      </material-card>
                    </v-card>
                    </v-flex>
                    </v-dialog>
                  <v-dialog  class="accept_bk" v-model="notifyAdmin" max-width="600">
                    <v-flex
                      xs12
                      md12
                    >
                    <v-card height="320">
          <v-form>
            <v-container py-0>
              <v-layout wrap>
                <v-flex xs12>
                  <v-textarea
                    class="purple-input"
                    label="Admin Notification"
                    v-model="notifyAdminAction"
                    value=""
                    placeholder="Please describe what information is wrong"
                  />
                </v-flex>
                <v-flex
                  xs12
                  text-xs-center
                >
                  <v-btn
                    class="mx-0 font-weight-light"
                    color="success"
                    @click="notifyAdmin = false"
                  >
                    Notify Admin
                  </v-btn>
                </v-flex>
              </v-layout>
            </v-container>
          </v-form>
                    </v-card>
                    </v-flex>
                    </v-dialog>
                    <!-- <v-dialog  class="accept_bk" v-model="filledDoc" max-width="1200">
                    <v-flex
                      xs12
                      md12
                    >
                    <v-card id="test2">
                        <p class="title" style="padding-top: 20px" align="center">
    ERASMUS + LEARNING AGREEMENT FOR VET MOBILITY
</p>
<div width="666" style="margin: 0 auto">
<p>
    <strong>I. DETAILS ON THE PARTICIPANT</strong>
</p>
</div>
<div align="center">
    <table border="1" cellspacing="0" cellpadding="0" width="0">
        <tbody>
            <tr>
                <td width="666" valign="top">
                    <p>
                        Name of the participant: Adrian Baraqui Vega
                    </p>
                    <p>
                        Field of vocational education: 1021 - Community
                        Sanitation
                    </p>
                    <p>
                        Sending institution (name, address): Randers
                        Produktionshøjskole, Dalagervej 6, Midtjylland
                    </p>
                    <p>
                        8960 Randers SØ
                    </p>
                    <p>
                        Contact person (name, function, e-mail, tel): Henrik
Vahlsted Madsen, teacher,                        <a href="mailto:hma@ranpro.dk">hma@ranpro.dk</a>,
                    </p>
                    <p>
                        <em>004523393332</em>
                    </p>
                </td>
            </tr>
        </tbody>
    </table>
</div>
<p>
    <strong></strong>
</p>
<p>
    <strong>II. DETAILS OF THE PROPOSED TRAINING PROGRAMME ABROAD</strong>
</p>
<div align="center">
    <table border="1" cellspacing="0" cellpadding="0" width="0">
        <tbody>
            <tr>
                <td width="666" valign="top">
                    <p>
                        Receiving organisation (name address): Embassy of the
                        Republic of Moldova in the Federal Republic of Germany,
                        Gotlandstrasse, 16, Berlin, 10439 Germany
                    </p>
                    <p>
Contact Person (name, function, e-mail, tel):                        <a name="Text2"></a>Mr. Dan Macovenco, administrative
                        director
                    </p>
                </td>
            </tr>
        </tbody>
    </table>
</div>
<br>
<div align="center">
    <table border="1" cellspacing="0" cellpadding="0">
        <tbody>
            <tr>
                <td width="666" valign="top">
                    <p>
                        Planned dates of start and end of the placement period:
                        01/04-2019 – 12/04-2019
                    </p>
                </td>
            </tr>
        </tbody>
    </table>
</div>
<p>
    <strong>III. COMMITMENT OF THE PARTIES INVOLVED</strong>
</p>
<p>
    <strong>
        By signing this document, the participant, the sending institution and
        the receiving organisation
    </strong>
(    <em>and the intermediary organisation if applicable)<strong>*</strong></em>
    <strong>
        confirm that they will abide by the principles of the Quality
        Commitment for VET Mobility projects attached below.
    </strong>
</p>
<p>
    *
    <em>
        please add a box below for the signature of the intermediary
        organisation – if applicable
    </em>
</p>
<div align="center">
    <table border="1" cellspacing="0" cellpadding="0">
        <tbody>
            <tr>
                <td width="566" valign="top">
                    <p>
                        <strong>THE PARTICIPANT </strong>
                    </p>
                    <p>
                        Participant’s signature
                    </p>
                    <p>
                        ...........................................................................
                        Date: 12/04-2019
                    </p>
                </td>
            </tr>
        </tbody>
    </table>
</div>
<br>
<div align="center">
    <table border="1" cellspacing="0" cellpadding="0">
        <tbody>
            <tr>
                <td width="566" colspan="2" valign="top">
                    <p>
                        <strong>THE SENDING INSTITUTION</strong>
                    </p>
                    <p>
                        We confirm that this proposed training programme
                        agreement is approved.
                    </p>
                    <p>
                        On completion of the training programme the institution
                        will issue a Europass Mobility to the participant
                    </p>
                </td>
            </tr>
            <tr>
                <td width="301" valign="top">
                    <p>
                        Coordinator’s signature
                    </p>
                    <p>
............................................................................                        <strong></strong>
                    </p>
                </td>
                <td width="264" valign="top">
                    <p>
                        <strong></strong>
                    </p>
                    <p>
                        Date: 12/04-2019<strong></strong>
                    </p>
                </td>
            </tr>
        </tbody>
    </table>
</div>
<br>
<div align="center">
    <table border="1" cellspacing="0" cellpadding="0">
        <tbody>
            <tr>
                <td width="566" colspan="2" valign="top">
                    <p>
                        <strong>THE RECEIVING ORGANISATION</strong>
                    </p>
                    <p>
                        We confirm that this proposed training programme is
                        approved.
                    </p>
                    <p>
                        On completion of the training programme the
                        organisation will issue a Certificate to the
                        participant.
                    </p>
                </td>
            </tr>
            <br>
            <tr>
                <td width="302" valign="top">
                    <p>
                        Coordinator’s signature
                    </p>
                    <p>
.............................................................................                        <strong></strong>
                    </p>
                </td>
                <td width="263" valign="top">
                    <p>
                        <strong></strong>
                    </p>
                    <p>
                        Date: 12/04-2019<strong></strong>
                    </p>
                </td>
            </tr>
        </tbody>
    </table>
</div>
<br>
<v-btn @click="down"></v-btn>
                    </v-card>
                    </v-flex>
                    </v-dialog> -->
  </v-container>
</template>

<script>
import { mapGetters } from 'vuex';
import { mapState } from 'vuex';
import store from '@/store';
import { db } from '@/main';
import jsPDF from 'jspdf';
import html2canvas from 'html2canvas';

export default {
  data() {
    return {
      video: './img/video_dummy.jpg',
      compLevel: [
        {
          i: 1,
          color: 'yellow',
          level: 'beginner',
          percent: '0%'
        },
        {
          i: 2,
          color: 'warning',
          level: 'experienced',
          percent: '25%'
        },
        {
          i: 3,
          color: 'success',
          level: 'competent',
          percent: '50%'
        },
        {
          i: 4,
          color: 'blue',
          level: 'knowledgeable',
          percent: '75%'
        },
        { i: 5,
          color: 'red',
          level: 'expert',
          percent: '100%'
        }
      ],
      userComps: null,
      doc: false,
      acceptedFlow: '',
      filledDoc: false,
      notifyAdmin: false,
      notifyAdminAction: ''
    }
  },
  computed: {
    ...mapGetters([
      'user'
    ]),
    comps() {
      return this.user.userDetails.data.competencies;
    },
    userDetails() {
      return this.user.userDetails.data;
    },
    compLVL() {
      const a = [];
      this.comps.forEach(c => {
          this.compLevel.forEach(cl => {
            if(c.level === cl.level) {
              a.push({
                color: cl.color,
                i: cl.i,
                level: cl.level,
                name: c.name,
                type: c.type
              })
            }
          })
        })
      return a;
    },
    PersonalType() {
      return this.compLVL.filter(c => {
        return c.type == "personal"
      })
    },
    VocationalType() {
      return this.compLVL.filter(c => {
        return c.type == "vocational"
      })
    },
    assignedFlow() {
      let f = this.user.flows
      let x = ''
      f.forEach(z => {
        if(z.flowName == this.userDetails.flow) {
          x = z
        }
      })
      return x
    }
  },
  methods: {
    upgrade(compName) {
      const userId = this.user.data.uid;
      let currLvl = this.compLVL;
      db.collection('users').where('uid', '==', userId).get().then((snapshot) => {
          let seif = null;
          snapshot.forEach(doc => {
            let competency = doc.data().competencies;
            competency.forEach((c) => {
              if(c.name == compName) {
                seif = c
                if(seif) { 
                  let a = currLvl.findIndex(el => el.name == seif.name)
                  let z = seif.i + 1
                  if(z < 6) {
                    let p = this.compLevel.find(el => el.i == z)
                    currLvl[a] = {
                      i: p.i,
                      name: seif.name,
                      level: p.level,
                      color: p.color,
                      type: seif.type
                    }
                    db.collection('users').doc(doc.id).update({
                      competencies: currLvl
                    }).then((data) => {
                      this.$store.dispatch('updateComps', currLvl)
                    })
                  } else {
                    console.log('already at max')
                  }
                } else {
                  console.log('nam seif')
                }
              }
            })
          })
      })
    },
    datez() {
      let plm = new Date().toISOString().substr(0, 10)
      console.log(plm)
    },
    acceptFlow() {
      const userId = this.user.data.uid;
      let now = new Date().toISOString().substr(0, 10);
      db.collection('users').where('uid', '==', userId).get().then((snapshot) => {
        snapshot.forEach(doc => {
          db.collection('users').doc(doc.id).update({
                      acceptedFlow: now
                    }).then(() => {
                      this.$store.dispatch('acceptFlow', now)
                    })
        })
      })
    },
    down() {
      const pdf = new jsPDF();
      // pdf.text("plm" + this.userDetails.flow,10,10);
      // pdf.html("<p>fdsfdsfs</p><h1>dasdas</h1>", {
      //   callback: () => {
      //     pdf.save('infso.pdf');
      //   }
      // });
      pdf.fromHTML(document.getElementById("test2"), 20,20,{'width':900});
      pdf.save('info.pdf')
    }
  }
}
</script>

<style>
.accept_bk {
  background: white
  
}
</style>>
