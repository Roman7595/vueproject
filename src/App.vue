<script>
// fix:double simb


export default{
    data(){
      return{
        msg:'',
        res: '',
        firstThreeRows :'*+/',
        lastThreeRows:'789-456.123',

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
            if (!this.isNumericChar(msg[i][0]) && msg[i].length>1){
              return ''
            }
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
          if(msg>=99999999999999999){
            
            this.res = "Too Much"
          }else if(msg !== 'NaN' && msg !== 'undefined'){
            this.res = msg
          } else{
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
      },
      backSpaceing(){
        this.msg = this.msg.substring(0,this.msg.length-1)
      }
      
    }

}
</script>

<template>
  <div class = 'main'>
    <div class='res'>{{ "Result: " + solver(msg) }}</div>
  <div class ='calc-box'>
    <div class = 'calc'>
    <input v-model="msg">
    
  
  <div class = 'nums-grid'>
  <button v-for = 'n in firstThreeRows' :key = 'n' :class = '"box-" + n +"-s"' class ='box' v-on:click="simbolInput(n)">{{n}}</button>
  <button v-on:click="backSpaceing()" class ='box'>←</button>
  <button v-for = 'n in lastThreeRows' :key = 'n' :class = '"box-" + n +"-s"' class ='box' v-on:click="simbolInput(n)">{{n}}</button>
  <button v-on:click="equal()" class = 'box equal'>=</button>
  <button key = '0' class ='box-0-s box' v-on:click="simbolInput(0)">{{0}}</button>
  </div>
  </div>
  </div>

  
  </div>
</template>

<style>
.res{
  border: 1px solid #000;
  margin: 20px;
  width: 100%;
}
  .calc{
    padding: 20px;
    background-color: #F3F3F3;
  }

  #app{
    display: flex;
    justify-content: center;
  }
  .main{
    width: 600px;
    height: 430px;
  }
  
  .calc-box{
    display: flex;
    justify-content: center;
  }

  .nums-grid{
    display: grid;
    width: 200px;
    aspect-ratio:4/5;
    grid-template-columns: auto auto auto auto;
    grid-template-rows: auto auto auto auto auto;
    gap: 5px;
  }
  
  
  input{
    width: 192px;
    height: 30px;
    font-size: 24px;
    margin-bottom: 10px;
  }
  .res{
    font-size: 36px;
    display: inline-block;
  }
  .box{
    background-color: #fff;
    border-radius: 4px;
    font-size: 20px;
    width: 46px;
    border: 1px solid #E5E5E5;
    color: #1A1A1A;
  }
  .box-0-s{
    width: 324%;

  }
  button:hover{
    background-color: #FCFCFC;
  }
  button:active {
    color: #5D5D5D;
    background-color: #F4F4F4;
  }
  .equal{
    height: 211%;
    background-color: #005A9E;
    color: #fff;
  }   
  .equal:hover{
    background-color: #196AA7;

  }
</style>