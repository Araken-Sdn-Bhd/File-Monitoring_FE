<template>
  <va-card class="col-span-12 sm:col-span-12">
    <va-card-title>Tender Management : View Record</va-card-title>
    <va-card-content>
      <form>
        <div class="grid grid-cols-12 gap-6">
          <div class="flex md:col-span-12 sm:col-span-6 col-span-12">
            <va-input v-model="title" placeholder="Enter Tender Title" />
          </div>
          <div class="flex md:col-span-6 sm:col-span-6 col-span-12">
              <va-select
                v-model="searchableSelectModel"
                :label="select"
                searchable
                text-by="description"
                track-by="id"
                :options="simpleOptions"
              />
          </div>
          <div class="flex md:col-span-6 sm:col-span-6 col-span-12">
            <va-input v-model="refNo" placeholder="Enter Tender Reference No." />
          </div>
          <div class="flex md:col-span-6 sm:col-span-6 col-span-12">
              <va-date-input
                v-model="simple"
                first-weekday="Monday"
                highlight-weekend
              />
            </div>
            <div class="flex md:col-span-6 sm:col-span-6 col-span-12">
            <va-input v-model="price" placeholder="Enter Submission Price (0.00)" />
          </div>
          <div class="flex md:col-span-12 sm:col-span-6 col-span-12">
            <va-input type="textarea" v-model="remark" placeholder="Enter Remark" max-rows="50" />
          </div>
         
        </div>
       
      </form>
    </va-card-content>

  </va-card>
  <br />
 
    <va-card class="col-span-2">
      <va-card-title>Tender Requirement</va-card-title>
      <va-card-content>
        <va-file-upload v-model="tenderRequirementFile" type="single" />
      </va-card-content>
    <br>
      <va-card-title>Technical Submission</va-card-title>
      <va-card-content>
        <va-file-upload v-model="technicalSubmissionFile" type="single" />
      </va-card-content>
    <br>
      <va-card-title>Financial Submission</va-card-title>
      <va-card-content>
        <va-file-upload v-model="financialSubmissionFile" type="single" />
      </va-card-content>
   <br>
      <va-card-title>Others Submission</va-card-title>
      <va-card-content>
        <va-file-upload v-model="othersSubmissionFile" type="single" />
      </va-card-content>
    </va-card>
    <br>
    <br>
    <br>

      <span><va-button icon="chevron_left" type="submit" @click="onBack()" color="#36e9f6"><Loader v-if="loader" />Previous Page</va-button></span>
      <span style="float:right"><va-button icon="check" type="submit" @click="onCreate()" color="success"><Loader v-if="loader" />Save Record</va-button></span>
  
  

 
</template>

<script>
import swal from 'sweetalert2';
import Loader from "../../components/loader.vue";
export default {
  components: { Loader },
  data() {
    return {
      loader:false,
      clientList: [],
      simple:new Date(),
      simpleOptions:[
      {
      id: 1,
      description: 'First option',
    },
    {
      id: 2,
      description: 'Second option',
    },
    {
      id: 3,
      description: 'Third option',
    },
      ],

      tenderRequirementFile:'',
      technicalSubmissionFile:'',
      financialSubmissionFile:'',
      othersSubmissionFile:'',
      
    };
  },
  mounted() {
   

  },
  beforeMount() {
    
    this.userdetails = JSON.parse(localStorage.getItem("userdetails"));
  
    
  },
  methods: {
   
    //add new record
    async onCreate() {
      swal.fire({
                  title: 'Do you want save this record?',
                  showCancelButton: true,
                  confirmButtonText: 'Save',
                }).then(async (result) => {
                if (result.isConfirmed) {
                  try {
                    this.loader = true;
                          const response = await this.$axios.post(
                              "tenderStore", {}, 
                          );
                          console.log("response", response.data);
                          if (response.data.code == 200) {
                            this.loader = false;
                              this.resetModel();
                              swal.fire('Record Successfully Saved', '', 'success')
                              this.GetClientList();
                          } else {
                            this.loader = false;
                              this.resetModel();
                              swal.fire({
                                  icon: 'error',
                                  title: 'Oops... Something Went Wrong!',
                                  text: 'the error is: ' + JSON.stringify(response.data.message),
                              })
                      
                          }
                  } catch (e) {
                          swal.fire({
                              icon: 'error',
                              title: 'Oops... Something Went Wrong!',
                              text: 'the error is: ' + e,
                          })
                      }
                } else if (result.isDismissed) {
                      swal.fire('Record are not saved', '', 'info')
                  }
              })
    },
    resetModel(){
      
    },
    //edit record
   onBack(){
    this.$router.push({ name: 'tender' });
   }
  
     
   

  }
};

</script>

<style scoped></style>
