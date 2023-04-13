<script lang="ts">
  import { error } from "@sveltejs/kit";

  interface User {
    name: {
      first: string;
      last: string;
    };
    email: string;
    cell: number;
    dob: {
      age: number;
    };
    login: {
      username: string;
    };
    info: {
      results: number;
    };
    registered: {
      age: number;
    };
    location: {
      street: {
        name: string;
        number: number;
      };
      country: string;
      city: string;
      postcode: number;
    };
    picture: {
      large: string;
    };
  }

  let users: Array<User> = [];

  async function fetchUsers() {
    try {
      const res = await fetch("https://randomuser.me/api/?results=32");
      const data = await res.json();
      users = data.results;

      if (!res.ok) {
        throw error(400, "Não foi possivel obter os dados.");
      }
    } catch (error) {
      console.log(`Erro: ${error}`);
    }
  }

  fetchUsers();

  // funções para estilo
  const theme: any = {
    light: "bg-white text-sky-700",
    dark: "bg-dark text-purple-700",
  };

  let currentTheme = "light";

  function toggleTheme() {
    currentTheme = currentTheme === "light" ? "dark" : "light";
  }
</script>

<!-- HTML -->

<div class="{theme[currentTheme]} bg-black box-border">
  <h1 class="text-center py-4 text-5xl">
    Collab Developers
    <button
      on:click={toggleTheme}
      class="hover:text-purple-600 transition-colors"
      ><i class="fa-solid fa-moon" /></button
    >
  </h1>

  <button />
  <nav>
    <ul class="flex justify-center">
      <li class="text-3xl px-3 hover:text-purple-600">
        <a href="https://www.colab.re/"><i class="fa-solid fa-globe" /></a>
      </li>
      <li class="text-3xl px-3 hover:text-purple-600">
        <a href="https://www.linkedin.com/company/colabapp"
          ><i class="fa-brands fa-linkedin" /></a
        >
      </li>
      <li class="text-3xl px-3 hover:text-purple-600">
        <a href="https://www.instagram.com/colabapp/"
          ><i class="fa-brands fa-square-instagram" /></a
        >
      </li>
      <li class="text-3xl px-3 hover:text-purple-600">
        <a href="https://twitter.com/colab_re"
          ><i class="fa-brands fa-twitter" /></a
        >
      </li>
      <li class="text-3xl px-3 hover:text-purple-600">
        <a href="https://www.youtube.com/colabapp"
          ><i class="fa-brands fa-youtube" /></a
        >
      </li>
    </ul>
  </nav>
  <div
    class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-3 gap-10 p-20"
  >
    {#each users as user}
      <div class="container">
        <div
          class="bg-slate-100 rounded-lg shadow-lg flex h-60 overflow-hidden cursor-pointer w-full"
        >
          <img
            class="w-60 box-border object-cover object-center gap-4"
            src={user.picture.large}
            alt="blog"
          />
          <div class="p-4">
            <h3 class="font-bold text-xl">
              {user.name.first}
              {user.name.last}, {user.dob.age}
            </h3>
            <p class="text-gray-600">
              <i class="fa-solid fa-location-dot" />
              {user.location.city}, {user.location.country}
            </p>
            <p class="text-gray-600">
              <i class="fa-brands fa-square-whatsapp" />
              {user.cell}
            </p>
            <p class="text-gray-600">
              <i class="fa-solid fa-envelope" />
              {user.email}
            </p>
            <div class="p-8 text-center">
              <i class="fa-brands fa-github" />
              /{user.login.username}
              <p>
                {user.location.street.number}
                <i class="fa-solid fa-code-commit" />
                |
                {user.registered.age} <i class="fa-solid fa-code-branch" /> |
                {user.dob.age} <i class="fa-solid fa-list-check" />
              </p>
            </div>
          </div>
        </div>
      </div>
    {/each}
  </div>
  <footer class="p-2 text-center">Colab © 2023</footer>
</div>

<!-- {#if users}
  <ul>
    {#each users as user}
      <li>
        <img src={user.picture.large} alt="profile pick" />
        <p>{user.name.first} {user.name.last}</p>
        <p>{user.location.city}, {user.location.country}</p>
      </li>
    {/each}
  </ul>
{:else}
  <p>Carregando usuários...</p>
{/if} -->
