---
title: Manuscripts - Bowen
display: Bowen
description: List of projects 
wrapperClass: 'text-center'
art: dots
projects:
  Current Focus:
    - name: 'Nuxt DevTools'
      link: 'https://github.com/nuxt/devtools'
      desc: 'Unleash Nuxt Developer Experience'
      icon: 'i-logos-nuxt-icon saturate-0'

    - name: 'Node Modules Inspector'
      link: 'https://github.com/antfu/node-modules-inspector'
      desc: 'Visualize and inspect your node_modules'
      icon: 'i-solar-black-hole-line-duotone'
    - name: 'VueUse'
      link: 'https://github.com/vueuse/vueuse'
      desc: 'Collection of Composition API utils for Vue 2 and 3'
      icon: 'vueuse'



<!-- @layout-full-width -->
<ListProjects :projects="frontmatter.projects" />
