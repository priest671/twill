<template>
  <a17-inputframe :error="error" :note="note" :label="label">
    <div class="singleselector">
      <div class="singleselector__grid">
        <div class="singleselector__item" v-for="radio in options">
          <input class="singleselector__radio" type="radio" :value="radio.value" :name="name" :id="name + '_' + radio.value" :disabled="radio.disabled || disabled" v-model="selectedValue">
          <label class="singleselector__label" :for="name + '_' + radio.value">{{ radio.label }}</label>
          <span class="singleselector__bg"></span>
        </div>
      </div>
    </div>
  </a17-inputframe>
</template>

<script>
  import FormStoreMixin from '@/mixins/formStore'
  import InputframeMixin from '@/mixins/inputFrame'

  export default {
    name: 'A17Singleselect',
    mixins: [InputframeMixin, FormStoreMixin],
    props: {
      name: {
        type: String,
        default: ''
      },
      selected: {
        default: ''
      },
      options: {
        default: function () { return [] }
      },
      disabled: {
        type: Boolean,
        default: false
      }
    },
    data: function () {
      return {
        value: this.selected
      }
    },
    computed: {
      selectedValue: {
        get: function () {
          return this.value
        },
        set: function (value) {
          this.value = value

          // see formStore mixin
          this.saveIntoStore()

          this.$emit('change', value)
        }
      }
    }
  }
</script>

<style lang="scss" scoped>
  @import '~styles/setup/_mixins-colors-vars.scss';

  .singleselector {
    color:$color__text;
    border:1px solid $color__border;
    background-clip: padding-box;
    box-sizing: border-box;
    overflow:hidden;
    border-radius:2px;
  }

  .singleselector__grid {
    display: flex;
    flex-direction: row;
    flex-wrap:wrap;
    box-sizing: border-box;
    overflow:hidden;
    margin-bottom: -1px;
    margin-right: -1px;
  }

  .singleselector__item {
    width:100%;
    height:50%;
    border-right:1px solid $color__border--light;
    border-bottom:1px solid $color__border--light;
    overflow: hidden;
    position:relative;

    @include breakpoint('small') {
      width:33.3333%;
    }

    @include breakpoint('medium') {
      width:100%;
    }

    @include breakpoint('large') {
      width:33.3333%;
    }

    @include breakpoint('large+') {
      width:25%;
    }
  }

  .singleselector__radio {
    position: absolute;
    width: 1px;
    height: 1px;
    margin-top: -1px;
    margin-left: -1px;
    padding: 0;
    border: 0 none;
    clip: rect(1px, 1px, 1px, 1px);
    overflow: hidden;
  }

  .singleselector__label {
    display: block;
    position: relative;
    padding-left: 15px + 18px + 15px;
    color: $color__text--light;
    cursor: pointer;
    z-index:1;
    height:50px;
    line-height:50px;
    white-space: nowrap;
    text-overflow: ellipsis;
    overflow:hidden;
    padding-right:5px;
  }

  .singleselector__label::before,
  .singleselector__label::after {
    content: '';
    position: absolute;
    left: 15px;
    top: 50%;
    margin-top:-9px;
    width: 18px;
    height: 18px;
    border-radius: 50%;
    transition: all .25s $bezier__bounce;
  }

  .singleselector__label::before {
    border: 1px solid $color__fborder;
    background-color: $color__f--bg;
  }

  .singleselector__label::after {
    border: 0 none;
    background-color: $color__icons;
    opacity:0;
    transform: scale(.1);
  }

  .singleselector__label:hover::before,
  .singleselector__radio:focus + .singleselector__label::before {
    border-color: $color__fborder--hover;
  }

  .singleselector__label:hover,
  .singleselector__radio:hover   + .singleselector__label,
  .singleselector__radio:focus   + .singleselector__label,
  .singleselector__radio:checked + .singleselector__label {
    color:$color__text;
  }

  .singleselector__radio:checked + .singleselector__label::before {
    border-color: $color__blue;
    background-color: $color__blue;
  }

  .singleselector__radio:checked + .singleselector__label::after {
    opacity: 1;
    transform: scale(.33);
    background-color: $color__background;
  }

  .singleselector__radio:disabled + .singleselector__label {
    opacity: .5;
    pointer-events: none;
  }

  .singleselector__radio:focus + .singleselector__label::before {
    border-color: $color__border--focus;
  }

  .singleselector__radio:focus:checked + .singleselector__label::before {
    border-color: $color__blue;
  }

  .singleselector__bg {
    display:block;
    position:absolute;
    top:0;
    left:0;
    right:0;
    bottom:0;
    z-index:0;
    background:$color__background;
    transition: background-color .25s $bezier__bounce;
  }

  .singleselector__radio:hover,
  .singleselector__radio:checked {
    + .singleselector__label + .singleselector__bg {
      background:$color__ultralight;
    }
  }
</style>