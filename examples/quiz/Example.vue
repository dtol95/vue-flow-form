 
<template>
  <div>
    <header class="vff-header">
      <div class="f-container">
        <!-- Add custom logo here -->
        <p>Royal Quiz</p>
      </div>
    </header>

    <flow-form
      ref="flowform"
      v-on:complete="onComplete"
      v-on:submit="onQuizSubmit"
      v-on:timer="onTimer"
      v-bind:questions="questions"
      v-bind:language="language"
      v-bind:standalone="true"
      v-bind:timer="true"
      timer-start-step="html_1"
    >
    <!-- Custom content for the Complete/Submit screen slots in the FlowForm component -->
      <!-- We've overriden the default "complete" slot content -->
      <template v-slot:complete>
        <p>
          <span class="fh2">You did it!</span>
          <span v-if="!submitted" class="f-section-text">
            Review your answers or press Calculate score to see your result.
          </span>
        </p>
      </template>

      <!-- We've overriden the default "completeButton" slot content -->
      <template v-slot:completeButton>
        <div class="f-submit" v-if="!submitted">
          <button
            class="o-btn-action"
            ref="button"
            type="submit"
            href="#"
            v-on:click.prevent="onQuizSubmit()"
            aria-label="Press to submit">
            <span>Calculate score</span>
          </button>
          <a 
            class="f-enter-desc"
            href="#"
            v-on:click.prevent="onQuizSubmit()"
            v-html="language.formatString(language.pressEnter)">
          </a>
        </div>
        <p class="text-success" v-if="submitted && time">Your time: {{ formattedTime }}</p>
        <p class="text-success" v-if="submitted && score < 4">"You scored {{ score }} out of {{ total }}. There's a lot of room for improvement."</p>
        <p class="text-success" v-else-if="submitted && score < 7">"You scored {{ score }} out of {{ total }}. Not bad at all!"</p>
        <p class="text-success" v-else-if="submitted && score <= total">"You scored {{ score }} out of {{ total }}. Wow, that's impressive!"</p>
      </template>
    </flow-form>
  </div>
</template>

<script>
  // Import necessary components and classes
  import FlowForm from '../../src/components/FlowForm.vue'
  import QuestionModel, { QuestionType, ChoiceOption, LinkOption } from '../../src/models/QuestionModel'
  import LanguageModel from '../../src/models/LanguageModel'
  // If using the npm package, use the following line instead of the ones above.
  // import FlowForm, { QuestionModel, QuestionType, ChoiceOption, LanguageModel } from '@ditdot-dev/vue-flow-form'
  export default {
    name: 'example',
    components: {
      FlowForm
    },
    data() {
      return {
        submitted: false,
        completed: false,
        score: 0, 
        total: 45, 
        time: 0,
        formattedTime: '',
        answers: {
          html_1: '3', // Founder of Royal Training Maps 
          html_2: '4', // Most likely your ex favorite Royal YouTuber 
          html_3: '2', // A legend of Royal Tutorials 
          html_4: '1', // Founder of Royal Training Maps 
          html_5: '2', // Raced to be the first gold crown 
          html_6: '3', // Team that won SR most of the times 
          html_7: '2', // Player who failed his own map and didn't qualify for his team during RNC 3 
          html_8: '4', // Event that never existed in Royal 
          html_9: '3', // Person who owes most of the records on Royal maps 
          html_10: '1', // The leading country in the number of records 
          html_11: '4', // Winner of one and only lucky Clover royal championship 
          html_12: '1', // Which team had won RNC 
          html_13: '4', // Creator of Royal Rumble competition 
          html_14: '2', // Royal mapper of the 2022 year 
          html_15: '3', // Royal photographer 
          question_16: [
            "lucian____",
            "lucian",
            "Lucian",
          ],
          question_17: [
            "Dog..",
            "Dog",
          ],
          question_18: [
            "Brimarine",
            "brimarine",
          ],
          question_19: [
            "yungweeb",
            "Yungweeb",
          ],
          question_20: [
            "Kelvawesome2070",
            "kelvawesome2070",
            "Kelvawesome",
          ],
          question_21: [
            "ya___",
            "Ya___",
            "ya",
          ],
          question_22: [
            "Sombot",
            "sombot",
          ],
          question_23: [
            "RafTorTV",
            "raftortv",
            "RafTor",
          ],
          question_24: [
            "Tchak77",
            "tchak77",
          ],
          question_25: [
            "Youpikaii",
            "youpikaii",
          ],
          question_26: [
            "kickz21",
            "Kickz21",
          ],
          question_27: [
            "TRASHER712",
            "trasher712",
          ],
          question_28: [
            "Rap10tor",
            "rap10tor",
          ],
          question_29: [
            "YourOnlyHope7",
            "youronlyhope7",
          ],
          question_30: [
            "Raway..",
            "raway",
          ],
          question_31: [
            "Moonub",
            "moonub",
          ],
          question_32: [
            "Surgetale",
            "surgetale",
          ],
          question_33: [
            "MrPaxi",
            "mrpaxi",
          ],
          question_34: [
            "YanMan",
            "yanman",
          ],
          question_35: [
            "Stormynoobgamer",
            "stormynoobgamer",
          ],
          question_36: [
            "Hey-E",
            "hey-e",
          ],
          question_37: [
            "Mabiere",
            "mabiere",
          ],
          question_38: [
            "Hayiom..",
            "hayiom",
          ],
          question_39: [
            "SkyRay746",
            "skyray746",
          ],
          question_40: [
            "Baupro32",
            "baupro32",
          ],
          question_41: [
            "LittleKryli___",
            "littlekryli___",
          ],
          question_42: [
            "betabr",
            "Betabr",
          ],
          question_43: [
            "Morgus_TM",
            "morgus_tm",
          ],
          question_44: [
            "Ideallinie13",
            "ideallinie13",
          ],
          question_45: [
            "Tona.",
            "tona",
          ],
        },
        language: new LanguageModel(),
        // Create question list with QuestionModel instances
        questions: [      
          new QuestionModel({
            id: 'start',
            tagline: '45 questions',
            title: 'How much do you know about Royal?',
            content: 'Let\'s start with some multiple choice questoins',
            type: QuestionType.SectionBreak,
            required: true,
          }),
          new QuestionModel({
            id: 'html_1',
            title: 'First player to get Master crown?',
            helpText: 'Master Crown = 1000 Wins',
            type: QuestionType.MultipleChoice,
            required: true,
            multiple: false,
            options: [
              new ChoiceOption({
                label: 'Intellectrick',
                value: '1'
              }),
              new ChoiceOption({
                label: 'Clover',
                value: '2'
              }),
              new ChoiceOption({
                label: 'Schwabba ',
                value: '3'
              }),
              new ChoiceOption({
                label: 'Maverick',
                value: '4'
              })
            ]
          }),
          new QuestionModel({
            id: 'html_2',
            title: 'Most likely your ex favorite Royal YouTuber?',
            helpText: '',
            type: QuestionType.MultipleChoice,
            required: true,
            multiple: false,
            options: [
              new ChoiceOption({
                label: 'Raway',
                value: '1'
              }),
              new ChoiceOption({
                label: 'RafTor',
                value: '2'
              }),
              new ChoiceOption({
                label: 'Chriso',
                value: '3'
              }),
              new ChoiceOption({
                label: 'Intellectrick ',
                value: '4'
              })
            ]
          }),
          new QuestionModel({
            id: 'html_3',
            title: 'A legend of Royal Tutorials?',
            helpText: '',
            type: QuestionType.MultipleChoice,
            required: true,
            multiple: false,
            options: [
              new ChoiceOption({
                label: 'Maverick',
                value: '1'
              }),
              new ChoiceOption({
                label: 'Chriso ',
                value: '2'
              }),
              new ChoiceOption({
                label: 'Intellectrick',
                value: '3'
              }),
              new ChoiceOption({
                label: 'Wirtual',
                value: '4'
              })
            ]
          }),
          new QuestionModel({
            id: 'html_4',
            title: 'Founder of Royal Training Maps?',
            helpText: '',
            type: QuestionType.MultipleChoice,
            required: true,
            multiple: false,
            options: [
              new ChoiceOption({
                label: 'Maverick ',
                value: '1'
              }),
              new ChoiceOption({
                label: 'Intellectrick',
                value: '2'
              }),
              new ChoiceOption({
                label: 'Dam_TM',
                value: '3'
              }),
              new ChoiceOption({
                label: 'Warl_TM',
                value: '4'
              })
            ]
          }),
          new QuestionModel({
            id: 'html_5',
            title: 'Raced to be the first gold crown?',
            helpText: '',
            type: QuestionType.MultipleChoice,
            required: true,
            multiple: false,
            options: [
              new ChoiceOption({
                label: 'Schwabba',
                value: '1'
              }),
              new ChoiceOption({
                label: 'Nsgr ',
                value: '2'
              }),
              new ChoiceOption({
                label: 'Wirtual',
                value: '3'
              }),
              new ChoiceOption({
                label: 'Intellectrick',
                value: '4'
              })
            ]
          }),
          new QuestionModel({
            id: 'html_6',
            title: 'Team that won the most Super Royals?',
            helpText: '',
            type: QuestionType.MultipleChoice,
            required: true,
            multiple: false,
            options: [
              new ChoiceOption({
                label: 'Dog/Trasher/Surgetale',
                value: '1'
              }),
              new ChoiceOption({
                label: 'Dam/Warl/Tchak',
                value: '2'
              }),
              new ChoiceOption({
                label: 'Intellectrick/Clover/Youpi ',
                value: '3'
              }),
              new ChoiceOption({
                label: 'Moonub/Chriso/Kickz',
                value: '4'
              })
            ]
          }),
          new QuestionModel({
            id: 'html_7',
            title: 'Player who failed his own map and didn\'t qualify for his team during RNC 3?',
            helpText: '',
            type: QuestionType.MultipleChoice,
            required: true,
            multiple: false,
            options: [
              new ChoiceOption({
                label: 'Maverick',
                value: '1'
              }),
              new ChoiceOption({
                label: 'Clover ',
                value: '2'
              }),
              new ChoiceOption({
                label: 'RafTor',
                value: '3'
              }),
              new ChoiceOption({
                label: 'SnappieTM',
                value: '4'
              })
            ]
          }),
          new QuestionModel({
            id: 'html_8',
            title: 'Event that never existed in Royal?',
            helpText: '',
            type: QuestionType.MultipleChoice,
            required: true,
            multiple: false,
            options: [
              new ChoiceOption({
                label: 'ROTD',
                value: '1'
              }),
              new ChoiceOption({
                label: 'RNC',
                value: '2'
              }),
              new ChoiceOption({
                label: 'RR',
                value: '3'
              }),
              new ChoiceOption({
                label: 'Snipe Me Friday ',
                value: '4'
              })
            ]
          }),
          new QuestionModel({
            id: 'html_9',
            title: 'Person who owes most of the records on Royal maps?',
            helpText: '',
            type: QuestionType.MultipleChoice,
            required: true,
            multiple: false,
            options: [
              new ChoiceOption({
                label: 'Warl_TM',
                value: '1'
              }),
              new ChoiceOption({
                label: 'Dog..',
                value: '2'
              }),
              new ChoiceOption({
                label: 'Dam_TM ',
                value: '3'
              }),
              new ChoiceOption({
                label: 'mtat_tm',
                value: '4'
              })
            ]
          }),
          new QuestionModel({
            id: 'html_10',
            title: 'The leading country in the number of records?',
            helpText: '',
            type: QuestionType.MultipleChoice,
            required: true,
            multiple: false,
            options: [
              new ChoiceOption({
                label: 'Germany ',
                value: '1'
              }),
              new ChoiceOption({
                label: 'France',
                value: '2'
              }),
              new ChoiceOption({
                label: 'Brazil',
                value: '3'
              }),
              new ChoiceOption({
                label: 'USA',
                value: '4'
              })
            ]
          }),
          new QuestionModel({
            id: 'html_11',
            title: 'Winner of one and only lucky Clover royal championship?',
            helpText: '',
            type: QuestionType.MultipleChoice,
            required: true,
            multiple: false,
            options: [
              new ChoiceOption({
                label: 'Raftor',
                value: '1'
              }),
              new ChoiceOption({
                label: 'Dog..',
                value: '2'
              }),
              new ChoiceOption({
                label: 'Trasher712',
                value: '3'
              }),
              new ChoiceOption({
                label: 'Doinksallday ',
                value: '4'
              })
            ]
          }),
          new QuestionModel({
            id: 'html_12',
            title: 'Which team had won RNC?',
            helpText: '',
            type: QuestionType.MultipleChoice,
            required: true,
            multiple: false,
            options: [
              new ChoiceOption({
                label: 'World ',
                value: '1'
              }),
              new ChoiceOption({
                label: 'Germany',
                value: '2'
              }),
              new ChoiceOption({
                label: 'USA',
                value: '3'
              }),
              new ChoiceOption({
                label: 'Poland',
                value: '4'
              })
            ]
          }),
          new QuestionModel({
            id: 'html_13',
            title: 'Creator of Royal Rumble competition?',
            helpText: '',
            type: QuestionType.MultipleChoice,
            required: true,
            multiple: false,
            options: [
              new ChoiceOption({
                label: 'RafTor',
                value: '1'
              }),
              new ChoiceOption({
                label: 'Yungweeb',
                value: '2'
              }),
              new ChoiceOption({
                label: 'Clover',
                value: '3'
              }),
              new ChoiceOption({
                label: 'Rulemaker ',
                value: '4'
              })
            ]
          }),
          new QuestionModel({
            id: 'html_14',
            title: 'Royal mapper of the 2022 year?',
            helpText: '',
            type: QuestionType.MultipleChoice,
            required: true,
            multiple: false,
            options: [
              new ChoiceOption({
                label: 'Clover',
                value: '1'
              }),
              new ChoiceOption({
                label: 'Maverick ',
                value: '2'
              }),
              new ChoiceOption({
                label: 'Zzork',
                value: '3'
              }),
              new ChoiceOption({
                label: 'Blurious',
                value: '4'
              })
            ]
          }),
          new QuestionModel({
            id: 'html_15',
            title: 'Royal photographer?',
            helpText: '',
            type: QuestionType.MultipleChoice,
            required: true,
            multiple: false,
            options: [
              new ChoiceOption({
                label: 'Dam_TM',
                value: '1'
              }),
              new ChoiceOption({
                label: 'Kira',
                value: '2'
              }),
              new ChoiceOption({
                label: 'Warl_TM ',
                value: '3'
              }),
              new ChoiceOption({
                label: 'RafTor',
                value: '4'
              })
            ]
          }),
          new QuestionModel({
            title: 'Excellent! Now for this part you will type out your answers, don\'t stress about capitalization',
            content: 'Names will not be used more than once, so if you have guessed someone already it will probably not be them if you are correct.',
            type: QuestionType.SectionBreak
          }),
          new QuestionModel({
            id: 'question_16',
            tagline: '',
            title: '4th place andy. Tchak\'s main simp.',
            type: QuestionType.Text,
            required: true,
            placeholder: 'Type your answer here...',
          }),

          new QuestionModel({
            id: 'question_17',
            tagline: '',
            title: 'Appreciates Moonub SadgeClap, made classic royal server',
            type: QuestionType.Text,
            required: true,
            placeholder: 'Type your answer here...',
          }),

          new QuestionModel({
            id: 'question_18',
            tagline: '',
            title: 'Main writer of the Royal community letter',
            type: QuestionType.Text,
            required: true,
            placeholder: 'Type your answer here...',
          }),

          new QuestionModel({
            id: 'question_19',
            tagline: '',
            title: 'This guy is always getting carried',
            type: QuestionType.Text,
            required: true,
            placeholder: 'Type your answer here...',
          }),

          new QuestionModel({
            id: 'question_20',
            tagline: '',
            title: 'First player to get the master crown fully on starter access',
            type: QuestionType.Text,
            required: true,
            placeholder: 'Type your answer here...',
          }),

          new QuestionModel({
            id: 'question_21',
            tagline: '',
            title: 'Has one of the shortest nicknames of the entire Royal community',
            type: QuestionType.Text,
            required: true,
            placeholder: 'Type your answer here...',
          }),

          new QuestionModel({
            id: 'question_22',
            tagline: '',
            title: 'The best Royal youngster',
            type: QuestionType.Text,
            required: true,
            placeholder: 'Type your answer here...',
          }),

          new QuestionModel({
            id: 'question_23',
            tagline: '',
            title: 'The one and only RRR creator/host',
            type: QuestionType.Text,
            required: true,
            placeholder: 'Type your answer here...',
          }),

          new QuestionModel({
            id: 'question_24',
            tagline: '',
            title: 'Announced retirement from Royal in 2022 but then made a comeback and started playing with one of the best teams',
            type: QuestionType.Text,
            required: true,
            placeholder: 'Type your answer here...',
          }),

          new QuestionModel({
            id: 'question_25',
            tagline: '',
            title: 'Toxic "LAYS" guy, but his English can make anyone smile',
            type: QuestionType.Text,
            required: true,
            placeholder: 'Type your answer here...',
          }),

          new QuestionModel({
            id: 'question_26',
            tagline: '',
            title: 'Chriso\'s rival',
            type: QuestionType.Text,
            required: true,
            placeholder: 'Type your answer here...',
          }),

          new QuestionModel({
            id: 'question_27',
            tagline: '',
            title: 'German pipes legend',
            type: QuestionType.Text,
            required: true,
            placeholder: 'Type your answer here...',
          }),

          new QuestionModel({
            id: 'question_28',
            tagline: '',
            title: 'RafTor\'s "alt" account',
            type: QuestionType.Text,
            required: true,
            placeholder: 'Type your answer here...',
          }),

          new QuestionModel({
            id: 'question_29',
            tagline: '',
            title: 'Fede 007 guy',
            type: QuestionType.Text,
            required: true,
            placeholder: 'Type your answer here...',
          }),

          new QuestionModel({
            id: 'question_30',
            tagline: '',
            title: 'A pro creator of Royal tutorials',
            type: QuestionType.Text,
            required: true,
            placeholder: 'Type your answer here...',
          }),

          new QuestionModel({
            id: 'question_31',
            tagline: '',
            title: 'Who???',
            type: QuestionType.Text,
            required: true,
            placeholder: 'Type your answer here...',
          }),

          new QuestionModel({
            id: 'question_32',
            tagline: '',
            title: 'Banana guy, who shared his advice on how to join the best team',
            type: QuestionType.Text,
            required: true,
            placeholder: 'Type your answer here...',
          }),

          new QuestionModel({
            id: 'question_33',
            tagline: '',
            title: 'A real Foxdrive friend',
            type: QuestionType.Text,
            required: true,
            placeholder: 'Type your answer here...',
          }),

          new QuestionModel({
            id: 'question_34',
            tagline: '',
            title: 'Likes to drive "inside of the tubes" even made a sequel of those maps',
            type: QuestionType.Text,
            required: true,
            placeholder: 'Type your answer here...',
          }),

          new QuestionModel({
            id: 'question_35',
            tagline: '',
            title: 'One of the PRIME-b players with a very long name',
            type: QuestionType.Text,
            required: true,
            placeholder: 'Type your answer here...',
          }),

          new QuestionModel({
            id: 'question_36',
            tagline: '',
            title: 'Also known as EAWS2929',
            type: QuestionType.Text,
            required: true,
            placeholder: 'Type your answer here...',
          }),

          new QuestionModel({
            id: 'question_37',
            tagline: '',
            title: 'EZ member who would like your beer',
            type: QuestionType.Text,
            required: true,
            placeholder: 'Type your answer here...',
          }),

          new QuestionModel({
            id: 'question_38',
            tagline: '',
            title: 'Used to have no bitches, but no friends',
            type: QuestionType.Text,
            required: true,
            placeholder: 'Type your answer here...',
          }),

          new QuestionModel({
            id: 'question_39',
            tagline: '',
            title: 'Prolific mapper with 1 SR win and 0 normal wins',
            type: QuestionType.Text,
            required: true,
            placeholder: 'Type your answer here...',
          }),

          new QuestionModel({
            id: 'question_40',
            tagline: '',
            title: 'Argentine RNC champion',
            type: QuestionType.Text,
            required: true,
            placeholder: 'Type your answer here...',
          }),

          new QuestionModel({
            id: 'question_41',
            tagline: '',
            title: 'Little guy who got returned to Royal because of Rick streams',
            type: QuestionType.Text,
            required: true,
            placeholder: 'Type your answer here...',
          }),

          new QuestionModel({
            id: 'question_42',
            tagline: '',
            title: '!jump',
            type: QuestionType.Text,
            required: true,
            placeholder: 'Type your answer here...',
          }),

          new QuestionModel({
            id: 'question_43',
            tagline: '',
            title: 'Old Royal boomer',
            type: QuestionType.Text,
            required: true,
            placeholder: 'Type your answer here...',
          }),

          new QuestionModel({
            id: 'question_44',
            tagline: '',
            title: 'The most active PRIME-ISSUE player',
            type: QuestionType.Text,
            required: true,
            placeholder: 'Type your answer here...',
          }),

          new QuestionModel({
            id: 'question_45',
            tagline: '',
            title: 'NADEO stuff person who is trying his best for us',
            type: QuestionType.Text,
            required: true,
            placeholder: 'Type your answer here...',
          }),

        ]
      }
    },
    methods: {
      /* eslint-disable-next-line no-unused-vars */
      onComplete(completed, questionList) {
        // This method is called whenever the "completed" status is changed.
        this.completed = completed
      },

      arrayEquals(a, b) {
        return Array.isArray(a) && Array.isArray(b) && a.length === b.length &&
          a.every((val, index) => val === b[index])
      },

      calculateScore() {
        this.questions.forEach(question => {
          if (question.type !== QuestionType.SectionBreak) {
            let answer = question.answer;

            if (typeof answer === 'string') {
              // Text-based question
              let correctAnswers = this.answers[question.id];

              // Check if the user's answer matches any of the correct variations
              if (correctAnswers.includes(answer.trim())) {
                this.score++;
              }
            } else if (typeof answer === 'object') {
              // Multiple-choice question
              answer.sort((a, b) => a - b);

              if (this.arrayEquals(answer, this.answers[question.id])) {
                this.score++;
              }
            }
          }
        });
      },

      onQuizSubmit() {
        // Set `submitted` to true so the form knows not to allow back/forward
        // navigation anymore.
        this.$refs.flowform.submitted = true
        
        this.submitted = true
        this.calculateScore()
      },

      onTimer(time, formattedTime) {
        this.time = formattedTime
        this.formattedTime = formattedTime
      }
    },
  }
</script>

<style lang="css">
  @import '../../src/assets/css/themes/theme-purple.css';
  /* If using the npm package, use the following lines instead of the one above */
  /* @import '~@ditdot-dev/vue-flow-form/dist/vue-flow-form.css'; */
  /* @import '~@ditdot-dev/vue-flow-form/dist/vue-flow-form.theme-purple.css'; */
</style>