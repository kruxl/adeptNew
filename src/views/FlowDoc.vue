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
                      md8
                    >
                    
                    <v-card ref="test2" id="test2">
                    <div>
                        <p class="title" align="center" style="padding-top: 20px">
    ERASMUS + LEARNING AGREEMENT FOR VET MOBILITY
</p>
<div width="666" style="margin: 0 auto">
<p>
    <strong>I. DETAILS ON THE PARTICIPANT</strong>
</p>
</div>
<div style="align:center; border:1px black solid">

                    <p>
                        Name of the participant: {{userDetails.firstName}} {{userDetails.lastName}}
                    </p>
                    <p>
                        Field of vocational education: {{userDetails.specialization}}
                    </p>
                    <p>
                        Sending institution (name, address): {{matchingFlow.teachers[0].school}}, {{matchingFlow.teachers[0].schoolAddress}}
                    </p>
                    <p>
                        Contact person (name, function, e-mail, tel): {{matchingFlow.teachers[0].name}}, teacher, {{matchingFlow.teachers[0].email}}, {{matchingFlow.teachers[0].phone}}
                    </p>

</div>
<p>
    <strong></strong>
</p>
<p>
    <strong>II. DETAILS OF THE PROPOSED TRAINING PROGRAMME ABROAD</strong>
</p>
<div style="align:center; border:1px black solid">
                    <p>
                        Receiving organisation (name address): {{matchingFlow.flowHC[0].name}}, {{matchingFlow.flowHC[0].address}}
                    </p>
                    <p>
Contact Person (name, function, e-mail, tel):                        <a name="Text2"></a> {{matchingFlow.flowHC[0].rep}}, {{matchingFlow.flowHC[0].email}}
                    </p>

</div>
<br>
<div style="align:center; border:1px black solid">

                    <p>
                        Planned dates of start and end of the placement period:
                        {{matchingFlow.startDate}} – {{matchingFlow.endDate}}
                    </p>

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
<div style="align:center; border:1px black solid">

                    <p>
                        <strong>THE PARTICIPANT </strong>
                    </p>
                    <p>
                        {{userDetails.firstName}} {{userDetails.lastName}} accepted on:
                    </p>
                    <p>
                        Date: {{userDetails.acceptedFlow}}
                    </p>

</div>
<br>
<div style="align:center; border:1px black solid">
 
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


                    <p>
                        Coordinator accepted on:
                    </p>
                    <p>
                        Date: {{matchingFlow.startDate}}<strong></strong>
                    </p>

</div>
<br>
<div style="align:center; border:1px black solid">

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
                    <p>
                        Coordinator accepted on:
                    </p>
                    <p>
                        Date: {{matchingFlow.startDate}}
                    </p>

</div>
<br>
</div>
</v-card>
</v-flex>
    </v-layout>
  </v-container>
</template>

<script>
import { mapGetters } from 'vuex';
import { mapState } from 'vuex';
import store from '@/store';
import { db } from '@/main';
import html2canvas from 'html2canvas';
import jsPDF from 'jspdf';
import VueHtml2Canvas from 'vue-html2canvas';
import rasterizeHTML from 'rasterizehtml';
import Vue from '@/main';

export default {
  data() {
    return {
      video: './img/video_dummy.jpg',
      output: null
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
    matchingFlow() {
        let f = this.user.flows.find(el => el.flowName == this.userDetails.flow)
        return f
    }
  },
  methods: {
    datez() {
      let plm = new Date().toISOString().substr(0, 10)
      console.log(plm)
    },
    // down() {

    // window.html2canvas = html2canvas
    // const pdf = new jsPDF();
//    let canvas = pdf.canvas;
//    let z = document.getElementById("test2")
    // html2canvas(z).then((canvas) => {
    //     console.log(canvas)
    // })

    //   pdf.fromHTML(document.getElementById("test2"), 1,1,);
    //   pdf.save('info.pdf')
    // pdf.html(document.getElementById("test2"), function () {
    //     pdf.save('Test.pdf');
    // });
        //     pdf.addHTML(document.getElementById("test2")).then(()=> {
        //     console.log("started");
        //     pdf.save('test.pdf')
        //     console.log("finished");
        // });

        
    // },
    down() {
			window.html2canvas = html2canvas //Vue.js 특성상 window 객체에 직접 할당해야한다.
			let that = this
			let pdf = new jsPDF('p', 'mm', 'a4')
			let canvas = pdf.canvas
			const pageWidth = 210 //캔버스 너비 mm
			const pageHeight = 295 //캔버스 높이 mm
			canvas.width = pageWidth
			let ele = document.getElementById("test2")
			let width = ele.offsetWidth // 셀렉트한 요소의 px 너비
			let height = ele.offsetHeight // 셀렉트한 요소의 px 높이
			let imgHeight = pageWidth * height/width // 이미지 높이값 px to mm 변환
			if(!ele){
				console.warn(selector + ' is not exist.')
				return false
			}
			html2canvas(ele, {
				onrendered: function(canvas) {
					let position = 0
					const imgData = canvas.toDataURL('image/png')
					pdf.addImage(imgData, 'png', 0, position, pageWidth, imgHeight, undefined, 'slow')
					//Paging 처리
					let heightLeft = imgHeight //페이징 처리를 위해 남은 페이지 높이 세팅.
					heightLeft -= pageHeight
					while (heightLeft >= 0) {
						position = heightLeft - imgHeight
						pdf.addPage();
						pdf.addImage(imgData, 'png', 0, position, pageWidth, imgHeight)
						heightLeft -= pageHeight
					}
					pdf.save('dasddd.pdf')
				},
			});	
		},
    abc() {
        const el = this.$refs.test2;
        const options = {
        type: 'dataURL'
        };
        this.output = this.$html2canvas(el, options)
    }
  }
}
</script>

<style>
.accept_bk {
  background: white
  
}
</style>>
