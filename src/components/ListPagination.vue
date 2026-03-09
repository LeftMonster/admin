<script setup lang="ts">
import { ref } from 'vue'
import { useI18n } from 'vue-i18n'
import { Button } from '@/components/ui/button'
import { Input } from '@/components/ui/input'

const props = defineProps<{
  page: number
  totalPage: number
  total: number
}>()

const emit = defineEmits<{
  changePage: [page: number]
}>()

const { t } = useI18n()
const jumpPage = ref('')

const jumpToPage = () => {
  if (!jumpPage.value) return
  const raw = Number(jumpPage.value)
  if (Number.isNaN(raw)) return
  const target = Math.min(Math.max(Math.floor(raw), 1), props.totalPage)
  if (target === props.page) return
  emit('changePage', target)
}
</script>

<template>
  <div
    v-if="totalPage > 1"
    class="flex flex-wrap items-center justify-between gap-3 border-t border-border px-6 py-4"
  >
    <div class="flex items-center gap-3">
      <span class="text-xs text-muted-foreground">
        {{ t('admin.common.pageInfo', { total, page, totalPage }) }}
      </span>
      <slot name="actions" />
    </div>
    <div class="flex flex-wrap items-center gap-2">
      <div class="flex items-center gap-2">
        <Input
          v-model="jumpPage"
          type="number"
          min="1"
          :max="totalPage"
          class="h-8 w-20"
          :placeholder="t('admin.common.jumpPlaceholder')"
        />
        <Button variant="outline" size="sm" class="h-8" @click="jumpToPage">
          {{ t('admin.common.jumpTo') }}
        </Button>
      </div>
      <div class="flex items-center gap-2">
        <Button
          variant="outline"
          size="sm"
          class="h-8"
          :disabled="page <= 1"
          @click="emit('changePage', page - 1)"
        >
          {{ t('admin.common.prevPage') }}
        </Button>
        <Button
          variant="outline"
          size="sm"
          class="h-8"
          :disabled="page >= totalPage"
          @click="emit('changePage', page + 1)"
        >
          {{ t('admin.common.nextPage') }}
        </Button>
      </div>
    </div>
  </div>
</template>
