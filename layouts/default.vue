<template>
  <div class="inv-layout-default">
    
  
      <InvHeaderBar />  
   
        <b-container class="inv-layout-main" fluid="lg">
          <b-row class="justify-content-md-center">
            <b-col cols="2">
            </b-col>
            <b-col cols="8" md="auto">
              <Nuxt />
            </b-col>
            <b-col  cols="2">
             
            </b-col >
          </b-row> 
        </b-container>

<InvFooterBar />

  </div>      
</template>

<script>
export default {
}
</script>


<style>
.inv-layout-main {
  min-height: 86.8vh;
 height: 100%;
}
</style>

