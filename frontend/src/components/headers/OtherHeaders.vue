<template>
  <div class="table-container">
    <H3>Other headers</H3>
    <FlattenHeaders v-bind:headers="otherHeaders" />
  </div>
</template>

<script lang="ts">
import { computed, defineComponent, PropType } from "@vue/composition-api";

import FlattenHeaders from "@/components/headers/FlattenHeaders.vue";
import H3 from "@/components/ui/h3.vue";
import { basicKeys, Header, HeaderItem, securityKeys } from "@/types";

export default defineComponent({
  name: "OtherHeaders",
  props: {
    header: {
      type: Object as PropType<Header>,
      required: true,
    },
  },
  components: { FlattenHeaders, H3 },
  setup(props) {
    const otherHeaders = computed(() => {
      const header = props.header.header;
      const keys = Object.keys(header);
      const otherKeys = keys
        .filter((key) => !key.startsWith("x-"))
        .filter((key) => securityKeys.indexOf(key) == -1)
        .filter((key) => basicKeys.indexOf(key) == -1);

      const items = otherKeys.map((key) => {
        if (key in header) {
          return { key: key, values: header[key] };
        }
      });

      return items.filter((x): x is HeaderItem => x !== undefined);
    });

    return { otherHeaders };
  },
});
</script>
