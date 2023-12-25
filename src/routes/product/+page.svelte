<script>
import { onMount } from 'svelte';
    import {fstore, db, app} from '../../firebase';
  
  import { collection, onSnapshot } from 'firebase/firestore';

  let products = [];

  const query = collection(db, 'products');

  onSnapshot(query, (snapshot) => {
    products = snapshot.docs.map(doc => doc.data());
  });
</script>

<!-- Your Svelte HTML template -->
<div class="relative overflow-x-auto shadow-md sm:rounded-lg">
  <table class="w-full text-sm text-left rtl:text-right text-gray-500 dark:text-gray-400">
    <!-- Table headers -->
    <thead class="text-xs text-gray-700 uppercase bg-gray-50 dark:bg-gray-700 dark:text-gray-400">
      <tr>
        <th scope="col" class="px-6 py-3">Product name</th>
        <th scope="col" class="px-6 py-3">Color</th>
        <th scope="col" class="px-6 py-3">Category</th>
        <th scope="col" class="px-6 py-3">Price</th>
        <th scope="col" class="px-6 py-3">Action</th>
      </tr>
    </thead>
    <!-- Table body -->
    <tbody>
      {#each products as product (product.id)}
        <tr class="odd:bg-white odd:dark:bg-gray-900 even:bg-gray-50 even:dark:bg-gray-800 border-b dark:border-gray-700">
          <th scope="row" class="px-6 py-4 font-medium text-gray-900 whitespace-nowrap dark:text-white">{product.name}</th>
          <td class="px-6 py-4">{product.color}</td>
          <td class="px-6 py-4">{product.category}</td>
          <td class="px-6 py-4">{product.price}</td>
          <td class="px-6 py-4">
            <a href="#" class="font-medium text-blue-600 dark:text-blue-500 hover:underline">Edit</a>
          </td>
        </tr>
      {/each}
    </tbody>
  </table>
</div>
