<template>
<div>
  <section class="user-info text-center">
    <div class="avator"><img :src="'static/img/chain/'+blockchain_lowercase+'_icon@2x.png'" width="100%"></div>
    <h4>{{blockchain}} Wallet</h4>
    <p id="copyContent">{{account}}</p>
    <div class="operation-list">
      <!--  -->
      <a class="grey-fsz" id="copyBtn" data-clipboard-target="#copyContent" data-clipboard-action="copy">{{$t("webwallet_copy_address")}}</a>
    </div>
    <div class="token-info" >
      <!--  -->
      <p>{{$t("webwallet_total")}} {{token}}</p>
      <h4>{{webUtil.addCommas(balances.sum,6)}} </h4>
      <p class="grey-fsz">≈ {{webUtil.addCommas(balances[unit])}} {{unit}}</p>
      <ul class="token-list">
        <li v-for="(item,index) in balances.list" :key="index">
          <div>{{$t(index)}} {{token}}</div>
          <div>{{item?webUtil.addCommas(item,6):0}}</div>
        </li>
      </ul>
    </div>
  </section>
  <div class="refresh-logout">
    <!-- -->
    <a class="grey-fsz refresh" @click="refresh">{{$t('webwallet_refresh')}}</a>
    <!-- -->
    <a class="grey-fsz logout" @click="logout">{{$t('webwallet_logout')}}</a>
  </div>
</div>
</template>

<script>
import common from 'static/js/common.js'
export default {
  props: ['account', 'balances','blockchain'],
  data() {
    return {
      unit:this.webCoin.unit,
      blockchain_lowercase:'',
      nodeUrl:'',
      network:'',
      token:''
    }
  },
  created() {
    this.getChain();
  },
  mounted() {
    this.copyAddress();
    this.getUnit();
  },
  methods: {
    refresh() {
      window.location.reload();
    },
    getChain(){
      if (this.blockchain) {
        let blockchain = this.blockchain.toLowerCase()
        this.blockchain_lowercase=blockchain
        this.nodeUrl = this.globalData[blockchain].nodeUrl
        this.network = this.globalData[blockchain].network
        this.token = this.globalData[blockchain].token
      }
    },
    logout() {
      this.webUtil.initMathExtension().then((res) => {
        return mathExtension.getIdentity(this.network);
      }).then((identity) => {
        mathExtension.forgetIdentity(this.network).then(() => {
          this.webUtil.setCookie("identity_"+this.blockchain_lowercase, '', {
            expires: -30,
            path: '/'
          });
          window.location.reload();
        });
      });
    },
    copyAddress() {
      var clipboard = new this.clipboard("#copyBtn");
      clipboard.on("success", (element=>{ 
        alert(this.$t('copy_success'))
      }));
      clipboard.on("error", (element=>{ 
        alert(this.$t('copy_fail'));
      }))
    },
    getUnit(){
      
      common.$on('val', (data) => {
        this.unit = data
      })
    }
  }
}
</script>
