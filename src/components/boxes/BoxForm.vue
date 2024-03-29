<template lang="pug">
div(:class="$style.cont")
  BoxBase(
    :editable="true",
    :modelValue="afState.title",
    @update:modelValue="updateTitle"
  )
    div(:class="$style.form")
      SwTypeForm(
        labelLeft="List",
        labelRight="Text",
        valLeft="list",
        valRight="text",
        @toggle="(val) => swFormType(val)"
      )
      Transition(name="disapear")
        InputAddList(
          v-if="afState.type === 'list'",
          :modelValue="afState.contentList",
          @update:modelValue="updateContentList"
        )
        textarea(
          v-else, 
          :class="$style.txtarea", 
          v-model="afState.content"
        )
      InputTagList(
        :modelValue="afState.tagList",
        @update:modelValue="updateTagList",
        placeholder="Introduzca las etiquetas"
      )
  div(:class="$style.btns")
    BtnIcon(
      label="Save",
      :icon="['fas', 'circle-check']",
      size="md",
      type="b",
      @click="handleSave"
    )
    patata
</template>

<script lang="ts">
import BoxBase from "./BoxBase.vue";
import SwTypeForm from "@/components/sw/SwTypeForm.vue";
import InputTagList from "@/components/InputTagList.vue";
import InputAddList from "@/components/InputAddList.vue";
import { BtnIcon } from "@/components/btn";
import { defineComponent, ref, PropType, watch } from "vue";
import { ArticleFormType, IArticleForm, toArticle } from "../../model";

export default defineComponent({
  components: {
    BoxBase,
    SwTypeForm,
    InputTagList,
    InputAddList,
    BtnIcon,
  },
  emit: ["save"],
  props: {
    article: {
      type: Object as PropType<IArticleForm>,
      required: false,
    },
  },
  setup(props, { emit }) {
    const af =
      props.article ||
      ({
        title: "",
        type: undefined,
        contentList: [],
        content: "",
        tags: [],
      } as IArticleForm);
    const isVisibleList = ref<boolean>(true);
    const afState = ref<IArticleForm>(af);
    const swFormType = (val: ArticleFormType) => {
      isVisibleList.value = val === "list";
      afState.value.type = val;
    };
    const handleDelete = () => {
      emit("delete");
    };
    const handleSave = () => {
      const articleSaved = toArticle(afState.value);
      emit("save", articleSaved);
    };
    watch(
      () => props.article,
      (act) => {
        swFormType(act?.type as ArticleFormType);
        afState.value = act as IArticleForm;
      }
    );
    const updateTitle = (newTitle: string) => {
      afState.value.title = newTitle;
    };
    const updateContentList = (newContentList: Array<string>) => {
      afState.value.contentList = newContentList;
    };
    const updateTagList = (newTagList: Array<string>) => {
      afState.value.tags = newTagList;
    };
    return {
      isVisibleList,
      afState,
      swFormType,
      handleDelete,
      handleSave,
      updateTitle,
      updateContentList,
      updateTagList,
    };
  },
});
</script>

<style lang="stylus" module>
@import "../../assets/base"
.cont
  flex-basis 900px
  .form
    display flex
    flex-direction column
    gap pd-md
    align-items start
    .txtarea
      font-size 18px
      border none
      border-radius br-sm
      box-sizing border-box
      background-color color-light
      padding pd-md
      width 100%
      min-height 130px
      &:focus
          outline 4px solid color-main
      &::placeholder
          color color-main
          font-style italic
  .btns
    margin-top pd-md
    display flex
    justify-content end
    gap pd-md
    flex-direction row-reverse
</style>
