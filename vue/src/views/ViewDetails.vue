<template>
  <div class="rome">
    <header class="header">
      
    <div class="profile">
        <img class="profile-picture" src="../assets/images/profile/charlesdeluvio-Mv9hjnEUHR4-unsplash.jpg" alt="">
        <div class="greeting">
            Welcome <span class="username">{{$store.state.user.username}}! </span>
            
        </div>
    </div>

       <h1 class="username">{{$store.state.user.username}}'s Itinerary Details</h1>
    </header>

    <div class="itinerary-details-header">
         <div class="itinerary-name-one">
             <div class="itinerary-details-header-data">Itinerary Name: </div> 
             <div class="itinerary-details-header-data-result">{{ItinerarysDetails[0].itineraryName}}</div>
         </div>
        <div class="itinerary-name-one">
            <div class="itinerary-details-header-data">Starting Point:</div> 
            <div class="itinerary-details-header-data-result">{{ItinerarysDetails[0].startingPoint}}</div>
        </div>
        <div class="itinerary-name-one">
            <div class="itinerary-details-header-data">Date:</div> 
            <div class="itinerary-details-header-data-result"> {{ItinerarysDetails[0].date}}</div>
        </div>

    </div>
   
    <br>

    <table class="table">
        <thead>
          <tr>
                <th>Landmark Name</th>
                <th>Venue Type</th>
                <th>City</th>
                <th>Country</th>
                <th>Address</th>
                <th>Go to Landmark</th>
                <th>Remove Landmark</th>
          </tr>
        </thead>
        <tbody>
            <tr  v-for="itinerary in ItinerarysDetails" v-bind:key="itinerary.itineraryId">
                <td class="itinerary-landmark"> {{itinerary.landmarkName}} </td>
                <td class="itinerary-venue"> {{itinerary.venueType}} </td>
                <td class="itinerary-city"> {{itinerary.cityName}} </td>
                <td class="itinerary-country"> {{itinerary.country}} </td>
                <td class="itinerary-address"> {{itinerary.address}} </td> 
                <td class="landmark-map">
                  <button type="submit" value="view-map" v-on:click="onClick(itinerary.landmarkId)">View Map</button>
                  </td>
                <td class="delete-landmark">
                  <button type="submit" value="delete" v-on:click="deleteLandMarkFromItinerary(itinerary.itineraryId, itinerary.landmarkId)">Remove</button>
                </td>             
            </tr>
        </tbody>
      </table>

      <br><br><br><br>

  <div class="add-new-landmark">
    

  </div>


    


    

    <main class="main">
      <br>
      <br>
      <br>
      <br>
      <div class="update-itinerary">
            <div class="update-button">
              <button class="update-button-button"><router-link v-bind:to="{ name: 'home' }" > Add New Landmark to Itinerary</router-link></button>
              <br>
              <button class="update-button-button" type="submit" value="Edit Itinerary Information" v-on:click="showForm = !showForm">Edit Itinerary Information</button>
              
          </div>
          <div class="form-card" v-show="showForm">
            <form v-on:submit.prevent="updateItinerary(itinerary)" >
                <div>
                    <label>Itinerary Name:</label>
                    <input type="text" v-model="itinerary.name" required />
                </div>
                <div>
                    <label>Starting Point:</label>
                    <input type="text" v-model="itinerary.startingPoint" required/>
                </div>
                <div>
                    <label>Itinerary Date</label>
                    <input type="date" v-model="itinerary.date" required/>
                </div>
                
                <!--- <button type="submit" class="button">Save Itinerary</button> --->
                
                <div class="update-button" >
                        <button class="update-button-button" type="submit" value="Update Itinerary">Update Itinerary</button>

                </div>
            </form>
      </div>
    </div>
      

     
    

    </main>

    <footer>
    
    </footer>
      

 


    
  </div>
</template>

<script>
import ItineraryService from '../services/ItineraryService';
//import LandmarkService from '../services/LandmarkService';




export default {
  name: "itinerary",
  props:["userId", "itineraryId", "landmarkId"
  ],
  data(){
    return {
    
      ItinerarysDetails:[

      ],

      landmarks: [],

      showForm : false,



         itinerary: {
                userId: this.$store.state.user.id,
                name: "",
                startingPoint: "",
                date: "",
            }
    }
  },
  created(){
    const itineraryId = this.$route.params.itineraryId
    ItineraryService.getItineraryDetailsByItineraryId(itineraryId).then((response) =>{
      this.ItinerarysDetails = response.data;
      console.log(response.data);
    })
    ItineraryService.getItineraryByItineraryId(itineraryId).then((response) => {

      this.itinerary = response.data;
    })


  },
  methods: {
    deleteLandMarkFromItinerary(itineraryId, landmarkId){
      ItineraryService.deleteLandMarkFromItinerary(itineraryId, landmarkId).then(() =>{

       /*       const userId = response.data.userId;
                const route = {
                    name: "profile",
                    params: {
                        
                        userid: userId
                    }
                };
                this.$route.push(route)*/

            const index = this. ItinerarysDetails.findIndex(i => i.landmarkId == landmarkId);
            this.ItinerarysDetails.splice(index, 1)

      })
    },
    updateItinerary(itinerary) {
      console.log(itinerary);
      ItineraryService.updateItinerary(itinerary).then((response) => {

        const userId = response.data.userId;
                const route = {
                    name: "profile",
                    params: {
                        
                        userid: userId
                    }
                };
                this.$router.push(route)

      })
    },
    onClick(landmarkId){
      //LandmarkService.getLandmarkById(landmarkId).then((response)=>{
       // this.landmarks = response.data
      
      //const landmarkId = this.ItinerarysDetails.landmarkId
     
      const route = {
        name: "view-map",
        params: {
          landmarkId: landmarkId
        }
     };
      this.$router.push(route)


    // })

    }
    
  }
};
</script>
<style scoped>
 
  .greeting{
    margin-top: 10px;
    font-size: 25px;
    margin-left: 10px;
  }

  img{
    width: 50px;
    height: 50px;
  }

  .profile{
    margin-top: 20px;
    display: flex;
    row-gap: 50px;
    background-color: white;
  }

  .itinerary-details-header{
      display: flex;
      flex-direction: column;
      margin-left: auto;
      margin-right: auto;
    
  }

  .itinerary-name-one{
      display: flex;
  }

  .itinerary-details-header-data{
      font-weight: bold;
      align-self: flex-start;
      font-size: 25px;
      
      
  }
  .itinerary-details-header-data-result{
      margin-left: 5px;
      font-size: 25px;
      
  }
  .itinerary-landmark{
      font-weight: bold;
  }

  .form-card{
    display: flex;
    align-items: center;
    
}

input{
    display: block;
    height: 50px;
    width: 800px;
    background-color: rgba(255, 255, 255, 0.07);
    border-radius: 3px;
    padding: 0 10px;
    margin-bottom: 8px;
    font-size: 25px;
    font-weight: 300;
}

form{
    align-content: center;
    
}

label{
    font-size: 20px;
}
::placeholder{
    color: #474646;
    font-size: 25px;
}


.or {
    text-align: center;
}

h2 {
    text-align: center;
}
.form-body{
    display: flex;
    flex-direction: row;
 
}

a {
  text-decoration: none;
}

a:visited{
  color: rgb(58, 58, 58);
}

a:visited:hover{
  color: white;
}

.update-itinerary{
  display: inline-block;
  justify-content: center;
  
}

.update-button{
  display: flex;
  flex-direction: column;
}

button{
  cursor: pointer;
}

button:hover{
  background-color: black;;
  color: white;
}

main{
  display: flex;
  justify-content: center;
  margin-bottom: 200px
}

.update-button-button{
  height: 50px;

}

.username {
  text-transform: capitalize;
}




</style>