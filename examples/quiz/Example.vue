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
        total: 8, 
        time: 0,
        formattedTime: '',
        answers: {
          html_1: '2', 
          html_2: '2',
          html_3: '2', 
          html_4: '2',
          ux_1: '2',
          ux_2: '2',
          ux_3: '2',
          ux_4: '2',
        },
        language: new LanguageModel(),
        // Create question list with QuestionModel instances
        questions: [      
          new QuestionModel({
            id: 'start',
            tagline: '8 questions',
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
                label: 'Clloverr', 
                value: '2'
               }),
              new ChoiceOption({
                label: 'Schwabba', 
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
                label: 'Clloverr', 
                value: '2'
               }),
              new ChoiceOption({
                label: 'Schwabba', 
                value: '3'
              }), 
              new ChoiceOption({
                label: 'Maverick', 
                value: '4'
              })
            ]
          }),
          new QuestionModel({
            id: 'html_3',
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
                label: 'Clloverr', 
                value: '2'
               }),
              new ChoiceOption({
                label: 'Schwabba', 
                value: '3'
              }), 
              new ChoiceOption({
                label: 'Maverick', 
                value: '4'
              })
            ]
          }),
          new QuestionModel({
            id: 'html_4',
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
                label: 'Clloverr', 
                value: '2'
               }),
              new ChoiceOption({
                label: 'Schwabba', 
                value: '3'
              }), 
              new ChoiceOption({
                label: 'Maverick', 
                value: '4'
              })
            ]
          }),
          new QuestionModel({
            title: 'Excellent! You are halfway through.',
            content: 'Lets see how well you know your maps.',
            type: QuestionType.SectionBreak
          }),
          new QuestionModel({
            id: 'ux_1',
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
                label: 'Clloverr', 
                value: '2'
               }),
              new ChoiceOption({
                label: 'Schwabba', 
                value: '3'
              }), 
              new ChoiceOption({
                label: 'Maverick', 
                value: '4'
              })
            ]
          }),
          new QuestionModel({
            id: 'ux_2',
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
                label: 'Clloverr', 
                value: '2'
               }),
              new ChoiceOption({
                label: 'Schwabba', 
                value: '3'
              }), 
              new ChoiceOption({
                label: 'Maverick', 
                value: '4'
              })
            ]
          }),
          new QuestionModel({
            id: 'ux_3',
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
                label: 'Clloverr', 
                value: '2'
               }),
              new ChoiceOption({
                label: 'Schwabba', 
                value: '3'
              }), 
              new ChoiceOption({
                label: 'Maverick', 
                value: '4'
              })
            ]
          }),
          new QuestionModel({
            id: 'ux_4',
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
                label: 'Clloverr', 
                value: '2'
               }),
              new ChoiceOption({
                label: 'Schwabba', 
                value: '3'
              }), 
              new ChoiceOption({
                label: 'Maverick', 
                value: '4'
              })
            ]
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