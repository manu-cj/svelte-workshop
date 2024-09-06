# svelte-workshop ToDo List Application

## Description

Ce projet consiste à créer une application de gestion de tâches (ToDo List) en utilisant SvelteKit. Les utilisateurs doivent se baser sur une maquette Figma pour réaliser l'interface utilisateur. L'application permet d'ajouter, modifier, et supprimer des tâches, tout en stockant les données localement dans le navigateur.

## Fonctionnalités

### Fonctionnalités principales
- **Fenêtre Modal** : Une fenêtre modal doit permettre d'ajouter une nouvelle tâche.
- **Local Storage** : Les tâches doivent être stockées dans le Local Storage pour persister les données après le rechargement de la page.
- **Gestion des tâches** :
  - Ajouter une tâche.
  - Modifier une tâche existante.
  - Supprimer une tâche.

### Must Have (Fonctionnalités supplémentaires)
- **Tri des tâches par nom** : Les utilisateurs doivent pouvoir trier les tâches par ordre alphabétique.
- **Tri des tâches par catégorie** : Les utilisateurs doivent pouvoir trier les tâches en fonction de leur catégorie.
- **Tri par état (complétée/non complétée)** : Les utilisateurs doivent pouvoir trier les tâches selon leur état (tâches complétées ou non complétées).

## Prérequis

Avant de commencer, assurez-vous d'avoir les éléments suivants installés sur votre machine :

- [Node.js](https://nodejs.org/) (version LTS recommandée)
- [SvelteKit](https://kit.svelte.dev/)

## Installation

 Pour créer un nouveau projet SvelteKit avec Vite, utilisez la commande suivante :

   ```bash
   npm create svelte@latest my-project
   ```

# Documentation des Méthodes et Fonctionnalités Svelte
N'hésitez pas à consulter la documentation officielle et à expérimenter avec les exemples disponibles sur leur site.

- [Documentation officielle](https://svelte.dev/docs/introduction)
- [Exemples](https://svelte.dev/examples/hello-world)

## Introduction

Svelte est un framework JavaScript moderne qui compile les composants en code JavaScript optimisé, ce qui améliore les performances. Cette documentation couvre les méthodes et fonctionnalités les plus courantes pour vous aider à utiliser Svelte efficacement.

## 1. Importer et Exporter des Composants

### Importer un Composant

Pour utiliser un composant dans un autre fichier, vous devez l'importer comme suit :

```svelte
<script>
  import MyComponent from './MyComponent.svelte';
</script>
```

### Exporter des props
Pour rendre une variable disponible pour les composants parents, utilisez export :


```svelte
<script>
  export let name = 'John Doe';
</script>

<p>Hello, {name}!</p>
```
### Réactivité

```svelte
<script>
  let count = 0;

  function increment() {
    count += 1;
  }
</script>

<button on:click={increment}>Count: {count}</button>
```
### Réactivité avec $: (Dépendances)
Utilisez $: pour créer des calculs réactifs basés sur d'autres variables :


```svelte
<script>
  let a = 1;
  let b = 2;
  $: sum = a + b;
</script>

<p>Sum: {sum}</p>

```
### Directives
#### if
Affiche du contenu conditionnellement :


```svelte
<script>
  let show = true;
</script>

{#if show}
  <p>This will be shown if `show` is true.</p>
{/if}


```
#### each 
Itère sur une liste d'éléments :


```svelte
<script>
  let items = ['Item 1', 'Item 2', 'Item 3'];
</script>

<ul>
  {#each items as item}
    <li>{item}</li>
  {/each}
</ul>

```

#### await
Gère les promesses et les états de chargement :


```svelte
<script>
  let promise = fetch('https://api.example.com/data').then(res => res.json());
</script>

{#await promise}
  <p>Loading...</p>
{:then data}
  <p>Data: {data}</p>
{:catch error}
  <p>Error: {error.message}</p>
{/await}

```

### Bindings
#### bind:value
Lie la valeur d'un champ de formulaire à une variable :


```svelte
<script>
  let name = '';
</script>

<input bind:value={name} placeholder="Enter your name" />
<p>Your name is {name}</p>

```
#### bind:group
Lie un ensemble de cases à cocher à une variable :


```svelte
<script>
  let selected = [];
</script>

<label>
  <input type="checkbox" bind:group={selected} value="Option 1"> Option 1
</label>
<label>
  <input type="checkbox" bind:group={selected} value="Option 2"> Option 2
</label>

<p>Selected options: {selected.join(', ')}</p>

```
### Événements
#### on:event
Déclenche une fonction lorsqu'un événement se produit :


```svelte
<script>
  function handleClick() {
    alert('Button clicked!');
  }
</script>

<button on:click={handleClick}>Click me</button>

```
#### dispatch
Émet des événements personnalisés à un composant parent :


```svelte
<script>
  import { createEventDispatcher } from 'svelte';
  const dispatch = createEventDispatcher();

  function notify() {
    dispatch('customEvent', { detail: 'Data from child' });
  }
</script>

<button on:click={notify}>Send Event</button>

```
#### event.detail
Accède aux données passées avec un événement personnalisé :



```svelte
<script>
  function handleCustomEvent(event) {
    console.log(event.detail);
  }
</script>

<ChildComponent on:customEvent={handleCustomEvent} />

```
### Stores
#### writable
Crée un store modifiable :


```svelte
<script>
  import { writable } from 'svelte/store';

  const count = writable(0);

  function increment() {
    count.update(n => n + 1);
  }
</script>

<button on:click={increment}>Increment</button>
<p>{$count}</p>

```
#### readable
Crée un store en lecture seule :


```svelte
<script>
  import { readable } from 'svelte/store';

  const time = readable(new Date(), set => {
    const interval = setInterval(() => set(new Date()), 1000);
    return () => clearInterval(interval);
  });
</script>

<p>The current time is {$time.toLocaleTimeString()}</p>

```
### Lifecycle Methods
#### onMount
Exécute une fonction lorsque le composant est monté dans le DOM :


```svelte
<script>
  import { onMount } from 'svelte';

  onMount(() => {
    console.log('Component mounted!');
  });
</script>

```
### beforeUpdate et afterUpdate
Exécute des fonctions avant et après que le composant soit mis à jour :


```svelte
<script>
  import { beforeUpdate, afterUpdate } from 'svelte';

  beforeUpdate(() => {
    console.log('Before update');
  });

  afterUpdate(() => {
    console.log('After update');
  });
</script>

```

#### onDestroy
Exécute une fonction lorsque le composant est détruit :

```svelte
<script>
  import { onDestroy } from 'svelte';

  onDestroy(() => {
    console.log('Component destroyed');
  });
</script>

```
