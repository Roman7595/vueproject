<script>



export default{
    data(){
      return{
        n1:'',
        n2:'',
        sn:'',
        msg:'',
        snaki:['-', '+', '/','*'],
        res: '',
        nums :'-+/*789.456m123'
      }
    },
    methods:{
      
      equal(){
        this.msg = this.res
      },

      isNumericChar(c) { 
        return /\d/.test(c) || c==='-' || c==='.'
      },

      // parenthesisDivision(msg){
      // let stackOfIndex = []
      // let msgLen = msg.length
      //   for (let i = 0; i<msgLen; i++){
      //     if (msg[i] === '('){
      //       stackOfIndex.push(i)
      //     }else if (msg[i] === ')'){
      //       const prevPos = stackOfIndex.pop()
      //       if (prevPos===undefined){
      //         return false 
      //       }
      //       const parsed = this.parseEq(msg.slice(prevPos+1,i))
      //       msg = msg.slice(0,prevPos) + this.rightOrderOperation(parsed) + msg.slice(i+1,msg.length)
      //       msgLen = msg.length
      //       i-= prevPos+1
      //     }
      //   }        
      //   if (stackOfIndex[0]){
      //     return false
      //   }
      //   if (msg){
      //     return msg
      //   }
        
      // },
      parseEq(m){
        let msg = m
        
        let isPrevNum = this.isNumericChar(msg[0]) || msg[0] === '−'
        let c = 0
        while (msg[0]==='−' || msg[0]==='-'){
          msg = msg.substring(1)
          c+=1
        }
        if (c%2 === 1){
          msg = '-' + msg
        }
        let parsedEq = []
        if (isPrevNum){
          parsedEq.push(msg[0])
          msg = msg.substring(1)
        }

        for (let i of msg){
          if(this.isNumericChar(i) === isPrevNum){
            parsedEq[parsedEq.length-1] += i
          }else{
            parsedEq.push(i)
          }
          isPrevNum = this.isNumericChar(i)
        }
        return parsedEq
      },
      
      rightOrderOperation(m){
        let msg = m
        let len = msg.length
        for (let i = 0; i<len; i++){
          
            switch (msg[i]){
              case '*':
                msg[i-1] = parseFloat(msg[i-1])*parseFloat(msg[i+1])
                msg.splice(i,2)
                i -= 2
                len = msg.length
                break
              case '/':
                msg[i-1] = parseFloat(msg[i-1])/parseFloat(msg[i+1])
                msg.splice(i,2)
                i -= 2
                len = msg.length
                break
            } 
        }
        len = msg.length
        for (let i = 0; i<len; i++){
            switch (msg[i]){
              case '+':
                msg[i-1] = parseFloat(msg[i-1]) + parseFloat(msg[i+1])
                msg.splice(i,2)
                i -= 2
                len = msg.length
                break
              case '−':
                msg[i-1] = parseFloat(msg[i-1]) - parseFloat(msg[i+1])
                msg.splice(i,2)
                i -= 2
                len = msg.length
                break
          }
        }
        return (+(+msg[0]).toFixed(15))+''

      },
      solver(m){
        let msg = m
        
        if (msg){
        msg = this.minusreplace(msg)
        msg = this.despacing(msg)
        }
        if(msg){
          // msg = this.parenthesisDivision(msg)
          if(msg){
          msg = this.parseEq(msg) 
          msg = this.rightOrderOperation(msg)
          if(msg !== 'NaN' && msg !== 'undefined'){
            this.res = msg
          }else{
            this.res = ''
          }
          }
          
        }else{
            this.res = ''
        }
        
        return this.res
      },
      despacing(msg){
        return msg.replace(/\s/g, '').replace(',','.');

      },
      minusreplace(msg){
        return msg.replaceAll('-','−')
      },
      simbolInput(text){
        this.msg+=text
      }
      
    }

}
</script>

<template>
  <div class = 'main'>
    <p class='res'>{{ solver(msg) }}</p>
  <div>
    
    <input v-model="msg">
    
  </div>
  <div class = 'nums-grid'>
  <button v-for = 'n in nums' :key = 'n' :class = '"box-" + n +"-s"' v-on:click="simbolInput(n)">{{n}}</button>
  <button v-on:click="equal()" class = 'equal'>=</button>
  <button key = '0' class ='box-0-s' v-on:click="simbolInput(0)">{{0}}</button>

  </div>
  

  
  </div>
</template>

<style>
  #app{
    display: flex;
    justify-content: center;
  }
  .box-0-s{
    width: 300%;
  }
  .nums-grid{
    display: grid;
    width: 200px;
    aspect-ratio:4/5;
    grid-template-columns: auto auto auto auto;
    grid-template-rows: auto auto auto auto auto;
    
    
  }
  .equal{
    height: 200%;
  }
  
  input{
    width: 192px;
  }
  .res{
    display: inline-block;
  }
  

</style>