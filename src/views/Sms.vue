<template>
    <div class="container py-4">
    <div class="p-5 mb-4 bg-light rounded-3">
      <div class="container-fluid py-5">
          <div class="card mx-auto">
            <div class="card-header text-white bg-dark">
              <h4>Sms Spoofing | Remaining: {{rateLimit}}</h4>
            </div>
            <div class="card-body">
<div class="alert alert-danger" role="alert">
  We sorry for limiting this service for 10 sms per user per day!
</div>                
                    <form @submit.prevent="sendSms">
                        <div class="form-group">
                            <label for="from">From</label>
                            <input id="from" type="text" placeholder="Facebook" name="from" v-model="from" class="form-control" required>
                        </div>
                        <div class="form-group">
                            <label for="to">To</label>
                            <input id="to" type="text" placeholder="66612400999" name="to" v-model="to" class="form-control" required>
                        </div>
                        <div class="form-group">
                            <label for="text">Text</label>
                            <input id="text" type="text" placeholder="Fuck you" name="text" v-model="text" class="form-control" required>
                        </div>                        
                        <input type="submit" class="btn btn-dark mt-2" value="Send">                     
                    </form>                                
            </div>
          </div>
      </div>
    </div>
  </div>
</template>

<script>
import { mapActions,mapGetters } from 'vuex'
import _ from 'lodash'
export default {
    data() {
        return {
            from: "",
            to: "",
            text: ""
        }
    },
    computed: {
        ...mapGetters(['error','authStatus', 'rateLimit'])
    },
    methods:{
        ...mapActions(['callSms']),
        sendSms(){
            let data = {
                from: this.from,
                to: this.to,
                text: this.text            
            }
            this.callSms(data).then(res => {
                
                if(!_.isNil(res)){
                    if(res.data.success){
                        this.from = ''
                        this.to = ''
                        this.text = ''
                        this.flashMessage.success({
                                title: 'Success',
                                message: this.authStatus
                            }); 


                    }
                }else{
                    this.flashMessage.error({
                        title: 'Error',
                        message: this.error
                    });
                }

            }).catch(err => {
                console.log(err)
            })   
                   

        }
    }
}
</script>

