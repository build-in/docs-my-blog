---
layout: page
---
<script setup>
import {
  VPTeamPage,
  VPTeamPageTitle,
  VPTeamMembers
} from 'vitepress/theme'

const members = [
 {
    avatar: 'https://img1.baidu.com/it/u=37551285,1810490177&fm=253&fmt=auto&app=120&f=JPEG?w=800&h=800',
    name: '周阳',
    title: 'Creator',
    links: [
      { icon: 'github', link: 'http://gitlab.payermax.inner/' },
      // { icon: 'twitter', link: 'https://twitter.com/youyuxi' }
    ]
  },
  {
    avatar: 'https://img0.baidu.com/it/u=2503234974,609436703&fm=253&fmt=auto&app=120&f=JPEG?w=800&h=800',
    name: '马优晨',
    title: 'Creator',
    links: [
        { icon: 'github', link: 'http://gitlab.payermax.inner/' },
        // { icon: 'twitter', link: 'https://twitter.com/youyuxi' }
    ]
 },
   {
    avatar: 'https://img2.baidu.com/it/u=242370523,787461913&fm=253&fmt=auto&app=120&f=JPEG?w=500&h=500',
    name: '刘虹邑',
    title: 'Creator',
    links: [
        { icon: 'github', link: 'http://gitlab.payermax.inner/' },
        // { icon: 'twitter', link: 'https://twitter.com/youyuxi' }
    ]
 },
    {
    avatar: 'https://img0.baidu.com/it/u=1933344541,3542066739&fm=253&fmt=auto&app=138&f=JPEG?w=500&h=500',
    name: '刘洪博',
    title: 'Creator',
    links: [
        { icon: 'github', link: 'http://gitlab.payermax.inner/' },
        // { icon: 'twitter', link: 'https://twitter.com/youyuxi' }
    ]
 }
]
</script>

<VPTeamPage>
  <VPTeamPageTitle>
    <template #title>
      Our Team
    </template>
    <template #lead>
      The development of the OMC document library is supported by a team, some of whose members are shown below.
    </template>
  </VPTeamPageTitle>
  <VPTeamMembers
    :members="members"
  />
</VPTeamPage>