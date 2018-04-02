<template>
  <div id="app">
    <Sign :textProp="signText"/>
    <transition-group name="flip-list" tag="ul" class="list-reset my-2">
        <Card v-for="(participant, pos) in participants"
          :key="participant.key"
          :name="participant.name"
          :imgSrc="participant.imgSrc"
          :action="remove.bind(this, pos)"/>
    </transition-group>
    <Button v-if="participants.length > 1 && running === false"
      :click="shuffle"
      :title="'Shuffle'"/>
    <Button v-if="running === false"
      :click="reload"
      :title="'reset'" />
    <Button v-if="participants.length > 1 && running === false"
      :click="eliminate.bind(1)"
      :title="'eliminate'"/>
    <Button v-if="participants.length > 1 && running === false"
      :click="eliminateAll"
      :title="'I\'m feeling lucky'"/>
  </div>
</template>

<script>
import Sign from './components/LightedSign';
import Card from './components/PortraitCard';
import Button from './components/Button';

function reload() {
  const participants = [
    {
      name: 'Misa',
      imgSrc: 'https://kyleshaver.s3.amazonaws.com/mesa2.JPG',
    },
    {
      name: 'Ethan',
      imgSrc: 'https://kyleshaver.s3.amazonaws.com/ethan.JPG',
    },
    {
      name: 'Kyle',
      imgSrc: 'https://kyleshaver.s3.amazonaws.com/kyle.JPG',
    },
    {
      name: 'Pete',
      imgSrc: 'https://kyleshaver.s3.amazonaws.com/pete.JPG',
    },
    {
      name: 'Rene',
      imgSrc: 'https://kyleshaver.s3.amazonaws.com/rene.JPG',
    },
    {
      name: 'Mario',
      imgSrc: 'https://kyleshaver.s3.amazonaws.com/mario.JPG',
    },
  ];
  for (let i = 0; i < participants.length; i += 1) {
    participants[i].key = Date.now() + i;
  }
  return JSON.parse(JSON.stringify(participants));
}
const participants = reload();

export default {
  name: 'App',
  components: {
    Sign,
    Card,
    Button,
  },
  data() {
    return {
      signText: this.getSignText(),
      participants,
      running: false,
    };
  },
  methods: {
    shuffle() {
      const a = this.participants;
      for (let i = a.length - 1; i > 0; i -= 1) {
        const j = Math.floor(Math.random() * (i + 1));
        [a[i], a[j]] = [a[j], a[i]];
      }
      this.participants = JSON.parse(JSON.stringify(this.participants));
    },
    eliminate(qty) {
      this.running = true;
      const self = this;
      const interval = setInterval(() => {
        self.shuffle.bind(self)();
      }, 100);
      setTimeout(() => {
        clearInterval(interval);
        if (self.participants.length > 1) {
          const removed = self.participants.pop();
          this.signText = this.getSafeText(removed.name);
        }
        if (qty > 1 && self.participants.length > 1) {
          setTimeout(() => {
            self.eliminate(qty - 1);
          }, 1000);
        } else {
          self.running = false;
        }
        if (self.participants.length === 1) {
          self.signText = this.getSignText(this.participants[0].name);
          self.running = false;
        }
      }, 1000);
    },
    eliminateAll() {
      this.eliminate(this.participants.length - 1);
    },
    reload() {
      const participantsRedo = reload();
      this.participants = participantsRedo;
      this.signText = this.getSignText();
    },
    remove(pos) {
      if (this.running === false) {
        if (this.participants.length > 1) {
          const removed = this.participants.splice(pos, 1)[0];
          this.signText = this.getSafeText(removed.name);
        }
        if (this.participants.length === 1) {
          this.signText = this.getSignText(this.participants[0].name);
        }
      }
    },
    getSignText(name) {
      if (name === undefined) {
        return 'whodunnit';
      }
      return `${name} dunnit`;
    },
    getSafeText(name) {
      return `${name}'s safe`;
    },
  },
};
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: hsl(210, 29%, 24%);
  padding: 30px 10px;
  height: 100%;
  width: 100%;
}
html {
  background: url('./assets/bg.jpg') no-repeat center center fixed;
  background-size: cover;
  -webkit-background-size: cover;
  -moz-background-size: cover;
  -o-background-size: cover;
}
.flip-list-move {
  transition: transform 0.1s;
}
</style>
