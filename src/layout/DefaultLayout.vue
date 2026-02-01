<template>
  <main class="flex">
    <aside class="w-64 bg-base-100 border-r border-base-200 p-4 shrink-0">
      <div class="text-xl font-bold mb-4 text-base-content">Pdnode Todos</div>
      <!-- <nav>
        <ul class="flex flex-col gap-2 list-none p-0">
          <li class="p-2 hover:bg-gray-200 rounded cursor-pointer">全部任务</li>
          <li class="p-2 hover:bg-gray-200 rounded cursor-pointer">工作</li>
          <li class="p-2 hover:bg-gray-200 rounded cursor-pointer">个人</li>
        </ul>
      </nav> -->
    </aside>
    <div class="flex-1">
      <slot />
    </div>
  </main>

</template>