<script lang="ts" setup>
import { computed, inject } from "vue"
import { directive as vTippy } from "vue-tippy"
import type { Node, ViewOptions } from "@/interfaces"
import { ViewOptionsKey } from "@/symbols"
import { NodeProp } from "@/enums"
import useNode from "@/node"

import { FontAwesomeIcon } from "@fortawesome/vue-fontawesome"
import { faCircleXmark } from "@fortawesome/free-solid-svg-icons"

const viewOptions = inject(ViewOptionsKey) as ViewOptions

interface Props {
  node: Node,
  settings: boolean
}
const props = defineProps<Props>()

const { nodeName } = useNode(props.node, viewOptions)

const content = computed(() => {
  let value = `<i>${nodeName.value}</i> has been <b>disabled</b> via a <a href='https://www.postgresql.org/docs/current/runtime-config-query.html' target='_blank'>planner method configuration parameter</a> but has been used anyway.`
  if (props.settings) {
    value += ` <br>Use the <i>SETTINGS</i> option in your EXPLAIN command to get more details.`
  }
  return value
})

const tippyOptions = computed(() => ({
  content: content.value,
  allowHTML: true,
  interactive: true,
  appendTo: () => document.body,
}))
</script>
<template>
  <span
    v-if="node[NodeProp.DISABLED]"
    class="text-danger"
    v-tippy="tippyOptions"
  >
    <FontAwesomeIcon fixed-width :icon="faCircleXmark"
    ></FontAwesomeIcon>
  </span>
</template>
