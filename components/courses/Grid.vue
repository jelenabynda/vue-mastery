<template lang='pug'>
div
  .grid(v-if='courses' v-cloak)
    nuxt-link.grid-card.card(v-for='course, key, index in courses'
                             :key='course.id'
                             :to='link(course)')
      .card-body
        CourseList(:course='course' :account='account')
        CourseAction(:course='course')
  FakeList(v-else)
</template>

<script>
import CourseList from '~/components/courses/List'
import CourseAction from '~/components/courses/Actions'
import FakeList from '~/components/courses/FakeList'

export default {
  name: 'courses-grid',

  components: {
    CourseList,
    CourseAction,
    FakeList
  },

  props: {
    account: {
      type: Object,
      required: false
    },
    courses: {
      type: Object,
      required: false
    }
  },

  methods: {
    link (course) {
      // Check if there is lesson in the course
      if (course.lessonsCount) {
        let lessonSlug = course.lessons[0].slug
        try {
          // Get the lessons started
          let lessons = this.account.courses[course.slug].completedLessons
          // Get the last completed lesson
          for (let key in lessons) {
            if (lessons.hasOwnProperty(key) && lessons[key]) {
              if (course.lessons[key]) {
                lessonSlug = key
              } else {
                console.log('The last completed lesson doesn\'t belong to this course anymore')
              }
            }
          }
        } catch (error) {}
        // Transform to friendly url
        return `/courses/${course.slug}/${lessonSlug}`
      }
      return ''
    }
  }
}
</script>

<style lang='stylus' scoped>
.content
  color: $gray

.grid
  display grid

  +tablet-up()
    grid-template-columns repeat(2, 1fr)
    grid-column-gap 40px

  +laptop-up()
    grid-template-columns repeat(1, 1fr)

.grid-card
  display flex
  flex-direction column
  justify-content space-between
  cursor pointer
  margin-bottom ($vertical-space/2)
  color $black

  &:hover
    text-decoration none

.recommend-course-list .grid-card
  width 100%

  .media-block
    text-align center
    grid-row-gap ($vertical-space/3)
    margin-bottom ($vertical-space/3)
    grid-template-columns 1fr
    grid-template-areas 'media'\
                        'body'

  .actions
    width 100%
    margin-left 0
</style>
