<script>

/*good
parseEq
despac
isNumericChar*/

export default{
    data(){
      return{
        n1:'',
        n2:'',
        sn:'',
        msg:'',
        snaki:['-', '+', '/','*'],
        res: '',
        nums :'0123456789+−*/()'
      }
    },
    methods:{
      add(){
        switch(this.sn){
          case'+':
            return parseInt(this.n1) + parseInt(this.n2)
          case'-':
            return parseInt(this.n1) - parseInt(this.n2)
          case'*':
            return parseInt(this.n1) * parseInt(this.n2)
          case'/':
            return parseInt(this.n1) / parseInt(this.n2)
          
        }
        
      },
      equal(){
        this.msg = this.res
      },

      isNumericChar(c) { 
        return /\d/.test(c) || c==='-'
      },

      parenthesisDivision(msg){
      let stackOfIndex = []
      let msgLen = msg.length
        for (let i = 0; i<msgLen; i++){
          if (msg[i] === '('){
            stackOfIndex.push(i)
          }else if (msg[i] === ')'){
            const prevPos = stackOfIndex.pop()
            if (prevPos===undefined){
              return false 
            }
            const parsed = this.parseEq(msg.slice(prevPos+1,i))
            msg = msg.slice(0,prevPos) + this.rightOrderOperation(parsed) + msg.slice(i+1,msg.length)
            msgLen = msg.length
            i-= prevPos+1
          }
        }        
        if (stackOfIndex[0]){
          return false
        }
        if (msg){
          return msg
        }
        
      },
      parseEq(m){
        let msg = m
        
        let isPrevNum = this.isNumericChar(msg[0]) || msg[0] === '−'
        let c = 0
        while (msg[0]==='−' || msg[0]==='-'){
          msg = msg.substring(1)
          c+=1
        }
        if (c%2 === 1){
          msg = '-'+msg
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
                msg[i-1] = parseInt(msg[i-1])*parseInt(msg[i+1])
                msg.splice(i,2)
                i -= 2
                len = msg.length
                break
              case '/':
                msg[i-1] = parseInt(msg[i-1])/parseInt(msg[i+1])
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
                msg[i-1] = parseInt(msg[i-1]) + parseInt(msg[i+1])
                msg.splice(i,2)
                i -= 2
                len = msg.length
                break
              case '−':
                msg[i-1] = parseInt(msg[i-1]) - parseInt(msg[i+1])
                msg.splice(i,2)
                i -= 2
                len = msg.length
                break
          }
        }
        return msg[0]+''

      },
      solver(m){
        let msg = m
        if (msg){
        msg = this.minusreplace(msg)
        msg = this.despacing(msg)
        }
        if(msg){
          msg = this.parenthesisDivision(msg)
        }
        if(msg){
        msg = this.parseEq(msg)
        this.res = this.rightOrderOperation(msg)
        return this.res
        }
      },
      despacing(msg){
        return msg.replace(/\s/g, '');
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
  <div>
    <input v-model="msg">
    
  </div>
  <button v-on:click="equal()">equals</button>
  <button v-for = 'n in nums' v-bind:key = 'n' v-on:click="simbolInput(n)">{{n}}</button>
  

  <h1>{{ solver(msg) }}</h1>
</template>

<style ></style>