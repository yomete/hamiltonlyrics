<template>
  <transition name="fade">
    <div class="home" v-if="!secondplayer">
      <div class="inner">
        <h1>Do you know your Hamilton Lyrics?</h1>
        <p>Test your knowledge of Hamilton: An American Musical by guessing who sang what lyric.</p>
        <p>Invite a second player by sending them this link {{url}}.</p>
      </div>
    </div>
    <div class="play" v-if="secondplayer">
      <div>
        <div class="container hamilton--header--text">
          <h1>Do you know your Hamilton Lyrics?</h1>

          <div class="columns hamilton--inner">
            <div class="column is-half left">
              <p class="title">User 1</p>
              <p class="subtitle">Total Score: {{playerdata.one.score}}</p>
            </div>
            <div v-if="secondplayer" class="column is-half right">
              <p class="title">User 2</p>
              <p class="subtitle">Total Score: {{playerdata.two.score}}</p>
            </div>
          </div>

          <div class="hamilton--lyrics--text">
            <p>{{question.lyric}}
            </p>
            <div class="hamilton--answers">
              <a v-bind:class="{ 'wronganswer': hasAnswered && !item.correct, 'correctanswer': hasAnswered && item.correct}" @click="checkAnswer(item)" v-for="(item, index) in options">{{item.name}}</a>
            </div>
          </div>
        </div>
      </div>
    </div>
  </transition>
</template>

<script>
  import ChannelDetails from '@/components/ChannelDetails'
  const lyrics = [
    {
      lyric: 'When he was ten his father split, full of it, debt-ridden. Two years later, see Alex and his mother bed-ridden. Half-dead sittin\' in their own sick, the scent thick',
      options: [{name: 'Aaron Burr', correct: false}, {name: 'James Madison', correct: false}, {name: 'John Laurens', correct: false}, {name: 'Eliza Hamilton', correct: true}],
      answer: 'Eliza Hamilton'
    },
    {
      lyric: 'I am sailing off to London. I’m accompanied by someone who always pays. I have found a wealthy husband. Who will keep me in comfort for all my days. He is not a lot of fun, but there’s no one',
      options: [{name: 'Eliza', correct: false}, {name: 'Peggy', correct: false}, {name: 'Angelica', correct: true}, {name: 'Maria', correct: false}],
      answer: 'Angelica'
    },
    {
      lyric: 'Lord, show me how to. Say no to this. I don’t know how to. Say no to this',
      options: [{name: 'Alexander Hamilton', correct: true}, {name: 'Thomas Jefferson', correct: false}, {name: 'John Laurens', correct: false}, {name: 'James Madison', correct: false}],
      answer: 'Alexander Hamilton'
    },
    {
      lyric: 'And y’all look pretty good in ya’ frocks. How ‘bout when I get back, we all strip down to our socks?',
      options: [{name: 'Philip Hamilton', correct: true}, {name: 'George Eacker', correct: false}, {name: 'John Adams', correct: false}, {name: 'John Adams', correct: false}],
      answer: 'Philip Hamilton'
    },
    {
      lyric: 'And when you said “Hi,” I forgot my dang name',
      options: [{name: 'Maria', correct: false}, {name: 'Eliza', correct: false}, {name: 'Peggy', correct: false}, {name: 'Angelica', correct: true}],
      answer: 'Angelica'
    },
    {
      lyric: 'Thank you for all your service',
      options: [{name: 'Eliza', correct: true}, {name: 'Maria', correct: false}, {name: 'Peggy', correct: false}, {name: 'Angelica', correct: false}],
      answer: 'Eliza'
    },
    {
      lyric: 'And I’m never gonna stop until I make ‘em. Drop and burn ‘em up and scatter their remains, I’m',
      options: [{ name: 'Lafayette', correct: true }, { name: 'Laurens', correct: false }, { name: 'Eacker', correct: false }, {name: 'Adams', correct: false}],
      answer: 'Lafayette'
    },
    {
      lyric: 'Yeah, you know what? We can change that. You know why?',
      options: [{name: 'Jefferson', correct: true}, {name: 'Maria', correct: false}, {name: 'Peggy', correct: false}, {name: 'Angelica', correct: false}],
      answer: 'Jefferson'
    },
    {
      lyric: 'I survived, but I paid for it',
      options: [{name: 'Aaron Burr', correct: true}, {name: 'James Madison', correct: false}, {name: 'George Washington', correct: false}, {name: 'King George', correct: false}],
      answer: 'Aaron Burr'
    },
    {
      lyric: 'When you knock me down I get the fuck back up again!',
      options: [{name: 'Burr', correct: false}, {name: 'Lafayette', correct: false}, {name: 'Mulligan', correct: true}, {name: 'Laurens', correct: false}],
      answer: 'Mulligan'
    },
    {
      lyric: 'Why so sad? Remember we made an arrangement when you went away.Now you’re making me mad',
      options: [{name: 'Peggy Schuyler', correct: false}, {name: 'King George', correct: true}, {name: 'Eliza Hamilton', correct: false}, {name: 'Angelica Schuyler', correct: false}],
      answer: 'King George'
    },
    {
      lyric: 'It’s bad enough daddy wants to go to war',
      options: [{name: 'Peggy Schuyler', correct: true}, {name: 'King George', correct: false}, {name: 'Angelica Schuyler', correct: false}, {name: 'Eliza Hamilton', correct: false}],
      answer: 'Peggy Schuyler'
    },
    {
      lyric: 'When I was given my first command. I led my men straight into a massacre',
      options: [{name: 'John Adams', correct: false}, {name: 'James Madison', correct: false}, {name: 'John Laurens', correct: false}, {name: 'George Washington', correct: true}],
      answer: 'George Washington'
    },
    {
      lyric: 'Black and white soldiers wonder alike if this really means freedom',
      options: [{name: 'Burr', correct: false}, {name: 'Lafayette', correct: false}, {name: 'Mulligan', correct: false}, {name: 'Laurens', correct: true}],
      answer: 'Laurens'
    },
    {
      lyric: 'We lower our guns as he frantically waves a white handkerchief',
      options: [{name: 'Lafayette', correct: true}, {name: 'Burr', correct: false}, {name: 'Mulligan', correct: false}, {name: 'Laurens', correct: false}],
      answer: 'Lafayette'
    },
    {
      lyric: 'And so the American experiment begins. With my friends all scattered to the winds',
      options: [{name: 'Hamilton', correct: true}, {name: 'Lafayette', correct: false}, {name: 'Mulligan', correct: false}, {name: 'Laurens', correct: false}],
      answer: 'Hamilton'
    },
    {
      lyric: 'We will fight up close, seize the moment and stay in it. It’s either that or meet the business end of a bayonet. The code word is ‘Rochambeau,’ dig me?',
      options: [{name: 'Hamilton', correct: true}, {name: 'Lafayette', correct: false}, {name: 'Mulligan', correct: false}, {name: 'Jefferson', correct: false}],
      answer: 'Hamilton'
    },
    {
      lyric: 'An immigrant you know and love who’s unafraid to step in!',
      options: [{name: 'Burr', correct: true}, {name: 'Laurens', correct: false}, {name: 'Washington', correct: false}, {name: 'Mulligan', correct: false}],
      answer: 'Burr'
    },
    {
      lyric: 'I’m a trust fund, baby, you can trust me!',
      options: [{name: 'Burr', correct: true}, {name: 'Lafayette', correct: false}, {name: 'George', correct: false}, {name: 'Laurens', correct: false}],
      answer: 'Burr'
    },
    {
      lyric: 'You want a revolution? I want a revelation',
      options: [{name: 'Angelica', correct: true}, {name: 'Eliza', correct: false}, {name: 'Mulligan', correct: false}, {name: 'Laurens', correct: false}],
      answer: 'Angelica'
    },
    {
      lyric: 'You punched the bursar',
      options: [{name: 'Lafayette', correct: false}, {name: 'Peggy', correct: false}, {name: 'Laurens', correct: false}, {name: 'Burr', correct: true}],
      answer: 'Burr'
    }
  ]
  export default {
    name: 'home',
    data () {
      return {
        presenceid: null,
        hasAnswered: false,
        question: null,
        options: null,
        correctanswer: null,
        count: null,
        players: 1,
        secondplayer: false,
        playerdata: {
          one: {
            id: null,
            score: 0,
            userid: null
          },
          two: {
            id: null,
            score: 0,
            userid: null
          }
        },
        userid: null,
        url: null
      }
    },
    created () {
      this.fetchData()
    },
    methods: {
      fetchData () {
        this.presenceid = this.getUniqueId()
        if (!this.checkPresenceID()) {
          var separator = (window.location.href.indexOf('?') === -1) ? '?' : '&'
          window.location.href = window.location.href + separator + this.presenceid
        }
        this.url = window.location.href
        this.getNewQuestion()
        let channel = ChannelDetails.subscribeToPusher()
        channel.bind('pusher:member_added', members => {
          this.players += 1
          this.secondplayer = true
        })
        channel.bind('pusher:subscription_succeeded', members => {
          if (members.count === 1 && !this.playerdata.one.id) {
            this.playerdata.one.id = members.myID
            this.playerdata.one.userid = 1
            this.userid = 1
          } else if (members.count === 2) {
            this.secondplayer = true
            this.playerdata.two.id = members.myID
            this.playerdata.two.userid = 2
            this.userid = 2
          }
        })
        channel.bind('pusher:member_removed', member => {
          this.players -= 1
          if (member.count === 1) {
            this.secondplayer = false
          }
        })
        channel.bind('client-send', (data) => {
          if (this.userid === 1) {
            this.playerdata.two.score = data.data.two.score
          } else if (this.userid === 2) {
            this.playerdata.one.score = data.data.one.score
          }
        })
      },
      getUniqueId () {
        return 'id=' + Math.random().toString(36).substr(2, 8)
      },
      checkPresenceID () {
        let getQueryString = (field, url) => {
          let href = url ? url : window.location.href
          let reg = new RegExp('[?&]' + field + '=([^&#]*)', 'i')
          let string = reg.exec(href)
          return string ? string[1] : null
        }
        let id = getQueryString('id')
        return id
      },
      checkAnswer (item) {
        let channel = ChannelDetails.subscribeToPusher()
        this.hasAnswered = true
        if (item.name === this.correctanswer) {
          if (this.userid === 1) {
            this.playerdata.one.score += 10
          } else if (this.userid === 2) {
            this.playerdata.two.score += 10
          }
        } else {
          if (this.userid === 1) {
            this.playerdata.one.score = Math.max(0, this.playerdata.one.score -= 10)
          } else if (this.userid === 2) {
            this.playerdata.two.score = Math.max(0, this.playerdata.two.score -= 10)
          }
        }
        channel.trigger('client-send', {data: this.playerdata})
        this.count = 3
        let countdown = setInterval(() => {
          this.count -= 1
          if (this.count === 0) {
            clearInterval(countdown)
            this.getNewQuestion()
          }
        }, 1000)
      },
      getRandomQuestions (array, count) {
        let length = array.length
        let randomIndexes = []
        let randomItems = []
        let index, item

        count = count | 1

        while (count) {
          index = Math.floor(Math.random() * length)
          if (randomIndexes.indexOf(index) === -1) {
            count--
            randomIndexes.push(index)
          }
        }

        randomIndexes.forEach((index) => {
          item = array.slice(index, index + 1).pop()
          randomItems.push(item)
        })

        if (randomItems.length === 1) {
          return randomItems.pop()
        } else {
          return randomItems
        }
      },
      getNewQuestion () {
        let question = this.getRandomQuestions(lyrics, 1)
        this.question = question
        this.options = question.options
        this.correctanswer = question.answer
        this.hasAnswered = false
      }
    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .home {
    display: flex;
    align-items: center;
    justify-content: center;
    height: 100vh;
  }
  h1 {
    font-size: 3rem;
    font-weight: bold;
  }
  p {
    font-size: 1.5rem;
    margin: 0 0 20px 0;
  }
  .play--button {
    background-color: white;
    color: #7fd4d3;
    font-weight: bold;
    border-radius: 20px;
    letter-spacing: 1px;
    padding: 20px;
    transition: all .3s ease;
    text-shadow: 0 1px 3px rgba(36,180,126,.4);
    text-transform: uppercase;
    box-shadow: 0 4px 6px rgba(50,50,93,.11), 0 1px 3px rgba(0,0,0,.08);
  }
  .play--button:hover {
    background-color: white;
    color: #7fd4d3;
    transform: translateY(-1px);
    box-shadow: 0 7px 14px rgba(50,50,93,.1), 0 3px 6px rgba(0,0,0,.08);
  }
  .fade-enter-active, .fade-leave-active {
    transition: opacity .5s
  }
  .fade-enter, .fade-leave-to {
    opacity: 0
  }
  a {
    color: #fff;
  }
  p {
    color: #fff;
  }
  h1 {
    font-size: 3rem;
    font-weight: bold;
    text-align: center;
  }
  .fade-enter-active, .fade-leave-active {
    transition: opacity .5s
  }
  .fade-enter, .fade-leave-to /* .fade-leave-active in <2.1.8 */ {
    opacity: 0
  }
  .play--button {
    background-color: white;
    color: #7fd4d3;
    font-weight: bold;
    border-radius: 20px;
    letter-spacing: 1px;
    padding: 20px;
    transition: all .3s ease;
    text-shadow: 0 1px 3px rgba(36,180,126,.4);
    text-transform: uppercase;
    box-shadow: 0 4px 6px rgba(50,50,93,.11), 0 1px 3px rgba(0,0,0,.08);
    position: absolute;
    top: 20px;
    right: 20px;
    z-index: 5;
  }
  .play--button:hover {
    background-color: white;
    color: #7fd4d3;
    transform: translateY(-1px);
    box-shadow: 0 7px 14px rgba(50,50,93,.1), 0 3px 6px rgba(0,0,0,.08);
  }
  .hamilton--header--text {
    margin-top: 50px;
  }
  .hamilton--inner {
    margin-top: 20px;
  }
  .hamilton--inner .left{
    text-align: left;
  }
  .hamilton--inner .right{
    text-align: right;
  }
  .title {
    font-weight: bold;
  }
  .hamilton--lyrics--text {
    width: 600px;
    margin: 0 auto;
  }
  .hamilton--lyrics--text p {
    font-weight: bold;
  }
  .hamilton--answers a{
    display: block;
    border: 3px solid white;
    border-radius: 50px;
    margin: 20px auto;
    width: 500px;
    padding: 10px;
  }
  .wronganswer {
    background-color: #ec6969;
    border: none !important;
    opacity: 0.4;
    transition: background-color 0.5s ease;
  }
  .correctanswer {
    background-color: #00c4a7;
    border: none !important;
    transition: background-color 0.5s ease;
  }
</style>
