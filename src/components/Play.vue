<template>
  <transition name="fade">
    <div>
      <router-link class="is-pulled-right button is-primary play--button" to="/">Go Home</router-link>
      <div class="container hamilton--header--text">
        <h1>Hamilton Lyrics Expert</h1>

        <div class="columns hamilton--inner">
          <div class="column is-half left">
            <p class="title">User 1</p>
            <p class="subtitle">Total Score: {{yourscore}}</p>
          </div>
          <div class="column is-half right">
            <p class="title">User 2</p>
            <p class="subtitle">Total Score: {{opponentscore}}</p>
          </div>
        </div>

        <div class="hamilton--lyrics--text">
          <p>When he was ten his father split, full of it, debt-ridden
            Two years later, see Alex and his mother bed-ridden
            Half-dead sittin' in their own sick, the scent thick
          </p>
          <div class="hamilton--answers">
            <a>Aaron Burr</a>
            <a>James Madison</a>
            <a>John Laurens</a>
            <a>Eliza Hamilton</a>
          </div>
        </div>
      </div>
    </div>
  </transition>
</template>

<script>
  // import Pusher from 'pusher-js'

//  const pusher = new Pusher('26b7d8fa6aa64488853b', {
//    cluster: 'eu',
//    encrypted: true,
//    authEndpoint: 'http://localhost:5000/pusher/auth'
//  })
//  let channel = pusher.subscribe('presence-hamilton')
//
//  channel.bind('pusher:subscription_succeeded', () => {
//    var triggered = channel.trigger('client-hamilton', { message: 'data' })
//    console.log(triggered)
//  })
  const lyrics = [
    {
      lyric: 'When he was ten his father split, full of it, debt-ridden. Two years later, see Alex and his mother bed-ridden. Half-dead sittin\' in their own sick, the scent thick',
      options: ['Aaron Burr, James Madison, John Laurens, Eliza Hamilton'],
      answer: 'Eliza Hamilton'
    },
    {
      lyric: 'I am sailing off to London. I’m accompanied by someone who always pays. I have found a wealthy husband. Who will keep me in comfort for all my days. He is not a lot of fun, but there’s no one',
      options: ['Eliza, Peggy, Angelica, Maria'],
      answer: 'Angelica'
    },
    {
      lyric: 'Lord, show me how to. Say no to this. I don’t know how to. Say no to this',
      options: ['Alexander Hamilton, Thomas Jefferson, John Laurens, James Madison'],
      answer: 'Alexander Hamilton'
    },
    {
      lyric: 'And y’all look pretty good in ya’ frocks. How ‘bout when I get back, we all strip down to our socks?',
      options: ['James Madison, John Adams, Mulligan, Philip Hamilton'],
      answer: 'Philip Hamilton'
    },
    {
      lyric: 'And when you said “Hi,” I forgot my dang name',
      options: ['Eliza, Maria, Peggy, Angelica'],
      answer: 'Angelica'
    },
    {
      lyric: 'Thank you for all your service',
      options: ['Eliza, Maria, Peggy, Angelica'],
      answer: 'Eliza'
    },
    {
      lyric: 'And I’m never gonna stop until I make ‘em. Drop and burn ‘em up and scatter their remains, I’m',
      options: ['Eliza, Maria, Peggy, Angelica'],
      answer: 'Lafayette'
    },
    {
      lyric: 'Yeah, you know what? We can change that. You know why?',
      options: ['Eliza, Maria, Peggy, Angelica'],
      answer: 'Thomas Jefferson'
    },
    {
      lyric: 'I survived, but I paid for it',
      options: ['Aaron Burr, James Madison, George Washington, King George'],
      answer: 'Aaron Burr'
    },
    {
      lyric: 'When you knock me down I get the fuck back up again!',
      options: ['Burr, Lafayette, Mulligan, Laurens'],
      answer: 'Mulligan'
    },
    {
      lyric: 'Why so sad? Remember we made an arrangement when you went away.Now you’re making me mad',
      options: ['Peggy Schuyler, King George, Angelica Schuyler, Eliza Hamilton'],
      answer: 'King George'
    },
    {
      lyric: 'It’s bad enough daddy wants to go to war',
      options: ['Peggy Schuyler, King George, Angelica Schuyler, Eliza Hamilton'],
      answer: 'Peggy Schuyler'
    },
    {
      lyric: 'When I was given my first command. I led my men straight into a massacre',
      options: ['John Adams, James Madison, John Laurens, George Washington'],
      answer: 'George Washington'
    },
    {
      lyric: 'Black and white soldiers wonder alike if this really means freedom',
      options: ['Burr, Lafayette, Mulligan, Laurens'],
      answer: 'Laurens'
    },
    {
      lyric: 'We lower our guns as he frantically waves a white handkerchief',
      options: ['Burr, Lafayette, Mulligan, Laurens'],
      answer: 'Lafayette'
    },
    {
      lyric: 'And so the American experiment begins. With my friends all scattered to the winds',
      options: ['Hamilton, Lafayette, Mulligan, Laurens'],
      answer: 'Hamilton'
    },
    {
      lyric: 'We will fight up close, seize the moment and stay in it. It’s either that or meet the business end of a bayonet. The code word is ‘Rochambeau,’ dig me?',
      options: ['Hamilton, Lafayette, Mulligan, Jefferson'],
      answer: 'Hamilton'
    },
    {
      lyric: 'An immigrant you know and love who’s unafraid to step in!',
      options: ['Burr, Washington, Mulligan, Laurens'],
      answer: 'Burr'
    },
    {
      lyric: 'I’m a trust fund, baby, you can trust me!',
      options: ['Burr, Lafayette, George, Laurens'],
      answer: 'Burr'
    },
    {
      lyric: 'You want a revolution? I want a revelation',
      options: ['Burr, Eliza, Mulligan, Laurens'],
      answer: 'Angelica'
    },
    {
      lyric: 'You punched the bursar',
      options: ['Lafayette, Peggy, Laurens, Burr'],
      answer: 'Burr'
    }
  ]
  export default {
    name: 'play',
    data () {
      return {
        timeLeft: 10,
        yourscore: 0,
        opponentscore: 0,
        questionindex: 0,
        questions: lyrics,
        answers: Array(lyrics.length).fill('')
      }
    },
    created () {
      this.endGame()
    },
    methods: {
      playGame () {

      },
      endGame () {
        if (this.timeLeft === 0) {
          console.log('end of game')
        }
      }
    }
  }
</script>

<style scoped>
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
</style>
