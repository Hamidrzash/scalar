<script setup lang="ts">
import { ROUTES } from '@/constants'
import { useLayout } from '@/hooks'
import { ScalarIcon } from '@scalar/components'
import { useRouter } from 'vue-router'

import DownloadAppButton from './DownloadAppButton.vue'
import SideHelp from './SideHelp.vue'
import SideNavGroup from './SideNavGroup.vue'
import SideNavRouterLink from './SideNavRouterLink.vue'

const { currentRoute } = useRouter()
const { layout } = useLayout()
</script>
<template>
  <nav
    aria-label="App Navigation"
    class="flex items-center justify-center sm:justify-between gap-1.5 app-drag-region pt-2"
    :class="
      layout === 'web' ? 'border h-header !pt-0' : 'sm:flex-col px-2 pb-2'
    "
    role="navigation">
    <SideNavGroup class="app-no-drag-region">
      <a
        class="hidden items-center mr-3 ml-1"
        :class="{
          'sm:flex': layout === 'web',
        }"
        href="https://www.scalar.com"
        target="_blank">
        <ScalarIcon
          icon="Logo"
          size="xl" />
      </a>
      <li
        v-for="({ icon, name, prettyName }, i) in ROUTES.filter(
          (route) => route.name !== 'settings',
        )"
        :key="i">
        <SideNavRouterLink
          :active="(currentRoute.name as string | undefined)?.startsWith(name)"
          :icon="icon"
          :name="name">
          {{ prettyName }}
        </SideNavRouterLink>
      </li>
    </SideNavGroup>
    <SideNavGroup class="app-no-drag-region">
      <li class="flex items-center">
        <SideNavRouterLink
          :active="currentRoute.name === 'settings'"
          icon="Settings"
          name="settings">
          Settings
        </SideNavRouterLink>
      </li>
      <li class="flex items-center">
        <SideHelp />
      </li>
      <li
        v-if="layout !== 'desktop'"
        class="hidden sm:ml-1.5 sm:flex items-center justify-center">
        <DownloadAppButton />
      </li>
    </SideNavGroup>
  </nav>
</template>
