<template>
  <div class="container">
    <h1>Alpha Tifo</h1>
    <p>Cet utilitaire permet de compter le <strong>nombre de lettres à tracer</strong> pour une banderole. Il tient en compte du fait
      que deux lettres se tracent qu'une seule fois. Il tient en compte aussi des numéros et signes de ponctuation et ne tiens pas en
      compte du fait qui il ait de majuscules/minuscules ou de caractères spéciaux (e devient E, ê devient E, É devient aussi E).</p>
    <textarea v-model="sentence" placeholder="Phrase à compter..." id="sentence" name="sentence" rows="5" cols="33"></textarea>
    <div class="buttons">
      <button class="primary" @click="count()">Compter les lettres</button>
      <button class="secondary" @click="reset()">Réinitialiser</button>
    </div>
    <ul>
      <li v-for="(count, character) in pairsCounts" :key="character">
        Caractère {{ character }} : {{ count }}
      </li>
    </ul>
  </div>
</template>

<script>

export default {
  name: 'AlphaTifo',
  mounted() {
    document.title = "Alpha Tifo";
  },
  data() {
    return {
      sentence: '',
      pairsCounts: {}
    };
  },
  methods: {
    reset() {
      this.sentence = '';
      this.pairsCounts = {};
    },
    normalizeCharacter(character) {
      const accentMap = {
        'À': 'A', 'à': 'a',
        'Â': 'A', 'â': 'a',
        'Ä': 'A', 'ä': 'a',
        'Æ': 'AE', 'æ': 'ae',
        'Ç': 'C', 'ç': 'c',
        'É': 'E', 'é': 'e',
        'È': 'E', 'è': 'e',
        'Ê': 'E', 'ê': 'e',
        'Ë': 'E', 'ë': 'e',
        'Î': 'I', 'î': 'i',
        'Ï': 'I', 'ï': 'i',
        'Ô': 'O', 'ô': 'o',
        'Œ': 'OE', 'œ': 'oe',
        'Ù': 'U', 'ù': 'u',
        'Û': 'U', 'û': 'u',
        'Ü': 'U', 'ü': 'u',
        'ÿ': 'y', 'Ÿ': 'Y',
        'Ñ': 'N', 'ñ': 'n',
      };
      return accentMap[character] || character;
    },
    count() {
      const normalizedSentence = this.sentence
          .toUpperCase()
          .split('')
          .map(this.normalizeCharacter)
          .filter(character => character !== '\n')
          .join('');
      const characterCounts = {};
      for (let i = 0; i < normalizedSentence.length; i++) {
        let character = normalizedSentence[i];
        if (character !== ' ') {
          if(character === ':') character = '.';
          if (characterCounts[character]) {
            characterCounts[character]++;
          } else {
            characterCounts[character] = 1;
          }
        }
      }
      const pairs = [];
      for (const character in characterCounts) {
        const count = characterCounts[character];
        const adjustedCount = count % 2 === 0 ? count : count + 1;
        const numberOfPairs = Math.floor(adjustedCount / 2);
        if (numberOfPairs > 0) {
          pairs.push([character, numberOfPairs]);
        }
      }
      pairs.sort((a, b) => a[0].localeCompare(b[0]));
      this.pairsCounts = {};
      for (const pair of pairs) {
        this.pairsCounts[pair[0]] = pair[1];
      }
    }
  }
}
</script>

<style>
body{
  margin: 0;
  background-color: #232b2b;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  display: flex;
  justify-content: center;
}

.container {
  max-width: 500px;
  box-sizing: border-box;
  padding: 60px 20px;
  display: flex;
  flex-direction: column;
  color: #ffffff;
}

h1{
  text-align: center;
  font-size: 44px;
  color: #e8e337;
}

textarea {
  padding: 10px;
  max-width: 100%;
  line-height: 1.5;
  border-radius: 5px;
  font-size: 14px;
  border: 1px solid #ccc;
  box-shadow: 1px 1px 1px #999;
  margin-bottom: 10px;
  resize: none;
}

textarea:focus,
textarea:hover {
  outline: none;
  border: 1px solid #e8e337;
}

li{
  margin: 5px 0;
}

.buttons{
  display: flex;
  gap: 10px;
}

button {
  border-radius: 8px;
  border-style: none;
  box-sizing: border-box;
  padding: 15px 10px;
  font-weight: 700;
  color: #232b2b;
  cursor: pointer;
  font-size: 14px;
  margin-bottom: 10px;
}

.primary{
  flex: 1;
  background-color: #e8e337;
  transition: 0.5s;
}
.primary:hover {
  background-color: #c4c01f;
}

.secondary{
  background-color: #d3d3d3;
  transition: 0.5s;
}
.secondary:hover {
  background-color: #afafaf;
}


</style>
