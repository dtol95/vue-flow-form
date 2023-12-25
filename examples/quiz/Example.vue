// Create and setup your form here
 
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
  /*
    Copyright (c) 2020 - present, DITDOT Ltd. - MIT Licence
    https://www.ditdot.hr/en
  */

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
        total: 15, 
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
        },
        language: new LanguageModel(),
        // Create question list with QuestionModel instances
        questions: [      
          new QuestionModel({
            id: 'start',
            tagline: '15 questions',
            title: 'How much do you know about Royal?',
            content: 'Test how well you know Royal history in the first quiz section.',
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
            title: 'Team that won SR most of the times?',
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
            title: 'Excellent! You are halfway through.',
            content: 'Lets see how well you know your maps.',
            type: QuestionType.SectionBreak
          })
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
            let answer = question.answer
            if (typeof answer === 'object') {
              answer.sort((a, b) => a - b)

              if (this.arrayEquals(answer, this.answers[question.id])) {
                this.score++
              }
            }
            if (answer == this.answers[question.id]) {
              this.score++
            }
          }
        })
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