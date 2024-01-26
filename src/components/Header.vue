<template>
  <div>
    <header class="flex justify-between border-b border-slate-300 px-10 py-8">
      <div class="flex items-center gap-4">
        <img src="/logo.png" alt="logo" class="w-10" />
        <div>
          <h2 class="text-xl font-bold uppercase">Sneakers Vue</h2>
          <p class="text-slate-500">Shop beast sneakers</p>
        </div>
      </div>

      <ul class="flex items-center gap-10">
        <li class="flex items-center gap-3 cursor-pointer text-gray-500 hover:text-black">
          <img src="/cart.svg" alt="Cart" />
          <b>120$</b>
        </li>

        <li class="flex items-center gap-3 cursor-pointer text-gray-500 hover:text-black">
          <img src="/heart.svg" alt="Favorites" />
          <span>Favorites</span>
        </li>

        <li class="flex items-center gap-3 cursor-pointer text-gray-500 hover:text-black">
          <img src="/profile.svg" alt="Add cart" />
          <span>Profile</span>
        </li>
      </ul>
    </header>
  </div>
</template>
