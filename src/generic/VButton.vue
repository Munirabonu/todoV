<script lang="ts">
import { defineComponent, PropType, computed, h } from 'vue';

export type VButtonSize = 'medium' | 'big' | 'huge'
export type VButtonColor =
  | 'primary'
  | 'info'
  | 'success'
  | 'warning'
  | 'danger'
  | 'white'
  | 'dark'
  | 'light'
export type VButtonDark = '1' | '2' | '3' | '4' | '5' | '6'

export default defineComponent({
  props: {
    href: {
      type: String,
      default: undefined,
    },
    icon: {
      type: String,
      default: undefined,
    },
    color: {
      type: String as PropType<VButtonColor>,
      default: undefined,
      validator: (value: VButtonColor) => {
        // The value must match one of these strings
        if (
          [
            undefined,
            'primary',
            'info',
            'success',
            'warning',
            'danger',
            'white',
            'dark',
            'light',
          ].indexOf(value) === -1
        ) {
          console.warn(
            `VButton: invalid "${value}" color. Should be primary, info, success, warning, danger, dark, light, white or undefined`
          )
          return false
        }

        return true
      },
    },
    bold: {
      type: Boolean,
      default: false,
    },
    light: {
      type: Boolean,
      default: false,
    },
    loading: {
      type: Boolean,
      default: false,
    },
    disabled: {
      type: Boolean,
      default: false,
    },
    static: {
      type: Boolean,
      default: false,
    },
  },
  setup(props, { slots, attrs }) {
    const classes = computed(() => {
      const defaultClasses: string[] = []
      return [
        ...defaultClasses,
        'button',
        'v-button',
        props.disabled && 'is-disabled',
        props.color && `is-${props.color}`,
        props.light && 'is-light',
        props.static && 'is-static',
        props.bold && 'is-bold',
      ]
    })
    const isIconify = computed(() => props.icon && props.icon.indexOf(':') !== -1)
    const getChildrens = () => {
      const childrens = []

      let iconWrapper
      if (isIconify.value) {
        const icon = h('i', {
          'aria-hidden': true,
          class: 'iconify',
          'data-icon': props.icon,
        })
        iconWrapper = h('span', { class: 'icon' }, icon)
      } else if (props.icon) {
        const icon = h('i', { 'aria-hidden': true, class: props.icon })
        iconWrapper = h('span', { class: 'icon' }, icon)
      }
      if (iconWrapper) {
        childrens.push(iconWrapper)
      }
      childrens.push(h('span', slots.default?.()))
      return childrens
    }

    return () => {
      return h(
        'button',
        {
          type: 'button',
          ...attrs,
          disabled: props.disabled,
          class: ['button', ...classes.value],
        },
        {
          default: getChildrens,
        }
      )
    }
  },
})
</script>

<style lang="css">
.button {
  border: none;
  padding: 5px 25px;
  border-radius: 5px;
  transition: all 0.7s ease;
  font-size: 0.95rem;
  color: var(--white);
  font-family:'Gill Sans', 'Gill Sans MT', Calibri, 'Trebuchet MS', sans-serif ;
}
.button:hover {
  opacity: 0.7;
}
.button.is-circle {
  border-radius: var(--radius-rounded);
}
.is-info {
  background-color: var(--info);
}
.is-warning {
  background-color: var(--warning);
}
.is-danger {
  background-color: var(--danger);
}
.is-success {
  background-color: var(--success);
}
.is-light {
  background-color: var(--light);
}
.is-primary {
  background-color: var(--primary);
}
.button.v-button:not([disabled]) {
  cursor: pointer;
}
</style>
