<script>
  // @ts-nocheck

  import { onMount } from "svelte";
  let ls = null;

  onMount(() => {
    ls = localStorage;

    if (ls.grades) {
      grades = JSON.parse(ls.grades);
      console.log(ls, ls.grades);
    } else
      grades = [
        { name: "Deutsch", grade: 15, weight: 1, id: 0 },
        { name: "Mathe", grade: 15, weight: 1, id: 1 },
        { name: "Englisch", grade: 15, weight: 1, id: 2 },
      ];
  });

  /**
   * @type {any[]}
   */
  export let grades = [];

  let idCounter = 99;

  function addGrade() {
    const newGrade = {
      name: "-",
      grade: 15,
      weight: 1,
      id: idCounter++,
    };
    grades = [...grades, newGrade];
  }

  /**
   * @type {Any}
   */
  $: points = (
    grades.reduce((acc, g) => acc + (g.grade - 0) * g.weight, 0) /
    grades.reduce((acc, g) => acc + g.weight, 0)
  ).toFixed(2);

  $: ls && ls.setItem("grades", JSON.stringify(grades));

  $: grade = ((17 - points) / 3).toFixed(2);
</script>

<svelte:head>
  <title>Home</title>
  <meta name="description" content="Svelte demo app" />
</svelte:head>

<section>
  <h1 class="font-black text-2xl p-5">Notenrechner</h1>

  <div class="p-5">
    <ul class="p-3 w-50 bg-slate-300 rounded shadow">
      {#each grades as { name, grade, weight, id }}
        <li
          class="flex justify-between items-center gap-2 bg-gray-200 p-2 rounded"
        >
          <p>
            <input
              class="p-1 rounded bg-gray-200 outline outline-slate-200"
              bind:value={name}
            />
          </p>
          <p>
            <input
              class="p-1 rounded bg-gray-200"
              type="number"
              max="15"
              min="0"
              bind:value={grade}
            /> pt.
          </p>
          <p>
            <input
              class="p-1 rounded bg-gray-200 w-8"
              type="number"
              bind:value={weight}
              min="0"
            />x
          </p>
          <button
            on:click={() => (grades = grades.filter((g) => g.id !== id))}
            class="text-sm text-red-500 cursor-pointer">X</button
          >
        </li>
      {/each}

      <button class="p-2 text-xl" on:click={addGrade}>+</button>
    </ul>

    <p class="py-2">
      Durchschnitt: <b>{points}</b> Punkte (Schnitt: <b>{grade}</b>)
    </p>
  </div>
</section>
