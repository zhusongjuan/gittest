<template>
	<div class="program" v-on:click="voteChange(index)" v-bind:class="{topLine: index + 1 < 3}">
    <div class="head icon"></div>
    <div class="body"><div>{{ data.name }}</div></div>
    <div class="foot icon"></div>

    <div class="actorLine">表演者：{{data.actors}}</div>
    <span></span>
    <div class="voteLine"><span class="left">给他投票</span><span class="select icon" v-bind:class="{selected: selected}"></span></div>
	</div>
</template>

<script>
import VOTE from '../../../api/vote.js'
import CommonUtil from '../../../util/util.js'

export default {
  name: 'program',
  props: ['data', 'index'],
  computed: {
    selected () {
      return this.$store.state.voteProList[this.index] === 1
    }
  },
  methods: {
    voteChange (proId) {
      var voteCount = 0
      var temp = this.$store.state.voteProList
      for (var i = 0; i < temp.length; i++) {
        voteCount += temp[i]
      }
      if (voteCount >= 3 && temp[proId] === 0) {
        CommonUtil.showToast('您只能投最多3票。')
        return
      }
      this.$store.commit('voteChange', proId)
      VOTE.vote(proId)
    }
  }
}
</script>

<style scoped>
	.program{padding-bottom: 30px;text-align: center;display: inline-block;width: 33%;color: red;font-weight: bold;font-size: 25px;}
  .topLine{margin-left: 11%;}
  .program .icon{background: url(../../../assets/icons.png) no-repeat; background-size: 200px 500px;}
	.program .head{background-position: -42px -319px;height: 75px;width: 118px;margin:auto;}
  .program .body{background: url(../../../assets/icon_bg.png) repeat-y;background-position: 0px 0px; height: 150px;background-size: 77px 25px;
    color:white;font-weight: bold;width: 80px;margin:auto;}
  .program .body div{position: relative;margin: 0 auto; writing-mode: vertical-rl;}
  .program .foot{background-position: -61px -455px;height: 30px;width: 80px;margin: auto;}

  .program .actorLine{margin-top: 15px;height: 60px;line-height: 30px; overflow: hidden;text-overflow: ellipsis;} 
  .program .voteLine .left{top:-25px;position: relative;}
	.program .select{background-position: -72px -76px;height: 67px;width: 57px;display: inline-block;}
  .program .selected{background-position: -72px -5px;}
</style>
