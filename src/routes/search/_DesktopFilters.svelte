<script>
  import Checkbox from "./../../components/ui/Checkbox.svelte";
  import Radio from "./../../components/ui/Radio.svelte";
  import { constructURL2 } from "./../../lib";
  import { goto, stores } from "@sapper/app";
  import { onMount } from "svelte";
  const { session, page } = stores();
  import { beforeUpdate, tick } from "svelte";
  export let facets = {},
    query = {}; // Required because after loading finished then only we will initiate the price slider component
  function clearFilters() {
    let url = "/search";
    goto(url);
  }
  function remove(k, i) {
    let ix = query[k].indexOf(i);
    query[k].splice(ix, 1);
    let url = constructURL("/search", query);
    goto(url);
  }
  function goCheckbox(e) {
    query[e.detail.model] = e.detail.selectedItems;
    let url = constructURL2("/search", query);
    goto(`${url}page=1}`);
  }
  let features = [
    "Processor Brand",
    "Processor Name",
    "Screen Size",
    "RAM",
    "Touchscreen",
    "RAM Type",
    "Screen Resolution",
    "SSD",
    "Processor Generation",
    "Keyboard",
    "Weight",
    "HDD Capacity",
    "Mic In",
    "Battery Backup",
    "Expandable Memory",
    "SSD Capacity",
    "Finger Print Sensor",
    "Backlit Keyboard",
    "NFC Support",
    "Face Recognition",
    "Optane Memory"
  ];
  function checkFeature(k) {
    return features.includes(k);
  }
  function stringToArray(v) {
    let a = (query[v.key] && query[v.key].split(",")) || [];
    return a;
  }
  // beforeUpdate(async () => {
  //   await tick();
  //   // array1.filter(value => array2.includes(value))
  //   let facets =  facets.features.name.buckets.filter(k=> features.includes(k.key))
  //   console.log('xxxxxxxxxxxxxxxxx',nr);
  // });
</script>

<div class="max-w-xs hidden md:block md:w-64">
  <div class="py-6">
    <div
      class="font-bold flex justify-between md:px-3 lg:px-4 items-center pt-3
      text-sm md:text-xs"
      style="margin-top:1px;">
      <div class="text-gray">FILTERS</div>
      <button
        on:click={clearFilters}
        class="text-right text-purple-500 cursor-pointer">
        CLEAR ALL
      </button>
    </div>

    <!-- {#each query as v, k}
    {#if v && v.length > 0 && k != 'page' && k != 'sort'}
      <div class="flex flex-wrap items-center text-xs">
        {#each v as i, ix}
          <div class="rnd-mnt">
            {i}
            <i
              class="fa fa-times cursor-pointer ml-1"
              aria-hidden="true"
              on:click={remove(k, i)} />
          </div>
        {/each}
      </div>
    {/if}
  {/each} -->
    <!-- {#if facets.categories && facets.categories.all.buckets && facets.categories.all.buckets.length > 0}
      <Checkbox
        items={facets.categories.all.buckets}
        title="CATEGORY"
        model="categories"
        selectedItems={query.categories || []}
        on:go={goCheckbox} />
    {/if} -->
    {#if facets.brands && facets.brands.all.buckets && facets.brands.all.buckets.length > 0}
      <Checkbox
        items={facets.brands.all.buckets}
        title="BRANDS"
        model="brands"
        selectedItems={query.brands || []}
        on:go={goCheckbox} />
    {/if}
    {#if facets.features && facets.features.name && facets.features.name.buckets && facets.features.name.buckets.length > 0}
      {#each facets.features.name.buckets as v, k}
        {#if checkFeature(v.key) && v.val && v.val.buckets && v.val.buckets.length > 0}
          <Checkbox
            items={v.val.buckets}
            title={v.key.toUpperCase()}
            model={v.key}
            selectedItems={stringToArray(v)}
            on:go={goCheckbox} />
        {/if}
      {/each}
    {/if}
    {#if facets.colors && facets.colors.colors && facets.colors.colors.name && facets.colors.colors.name.buckets && facets.colors.colors.name.buckets.length > 0}
      <Checkbox
        items={facets.colors.colors.name.buckets}
        title="COLOR"
        model="color"
        selectedItems={query.colors || []}
        on:go={goCheckbox} />
    {/if}
  </div>
</div>
