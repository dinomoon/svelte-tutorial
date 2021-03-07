# Svelte Tutorial - The Net Ninja

## What is svelte?

- `Compliler**` for creating reactive web apps & interfaces
- Can be used for small parts of a site, or entirely (SPA)

## Other framework differences

- Svelte is a compiler not a framework
- Svelte compiles your code for production at `build time` into a single, vanila, JavaScript bundle
- No extra scripts or libraries are shipped to production
- Often results in a faster running website

---

## Basic Syntax

### ◼ User input & Data binding

```html
<script>
  let name = 'Yoshi';
  let beltColor = 'black';

  const handleClick = () => {
    beltColor = 'orange';
  };

  const handleInput = (e) => {
    beltColor = e.target.value;
  };
</script>

// 밑의 두 코드는 같은 의미
<input type="text" on:input="{handleInput}" value="{beltColor}" />
<input type="text" bind:value="{beltColor}" />
```

<br />

### ◼ Reactive values & Reactive statements

- 값이 변하면 자동으로 업데이트(실행)해준다.

```html
<script>
  let firstName = 'Jimi';
  let lastName = 'Hendrix';
  let beltColor = 'black';

  $: fullName = `${firstName} ${lastName}`;
  $: {
    console.log(beltColor);
    console.log(fullName);
  }
</script>

<main>
  <p>{fullName} - {beltColor} belt</p>
  <input type="text" bind:value="{firstName}" />
  <input type="text" bind:value="{lastName}" />
  <input type="text" bind:value="{beltColor}" />
</main>
```

<br />

### ◼ Loops

```html
<script>
  let people = [
    { name: 'yoshi', beltColour: 'black', age: 25, id: 1 },
    { name: 'mario', beltColour: 'orange', age: 45, id: 2 },
    { name: 'luigi', beltColour: 'brown', age: 35, id: 3 },
  ];
</script>

<main>
  {#each people as person (person.id)}
  <div>
    <h4>{person.name}</h4>
    <p>{person.age} years old, {person.beltColour} belt.</p>
  </div>
  {:else}
  <p>There are no people to show...</p>
  {/each}
</main>
```

<br />

### ◼ Inline event handlers

```html
<script>
	let people = [
    { name: 'yoshi', beltColour: 'black', age: 25, id: 1 },
    { name: 'mario', beltColour: 'orange', age: 45, id: 2 },
    { name: 'luigi', beltColour: 'brown', age: 35, id: 3 }
  ];

	const handleClick = (id) => {
		// delete the person from people
		people = people.filter(person => person.id != id);
	}
</script>

<main>
	{#each people as person (person.id)}
		<div>
			<h4>{person.name}</h4>
			<p>{person.age} years old, {person.beltColour} belt.</p>
			<button on:click={() => handleClick(person.id)}>delete</button>
		</div>
	{:else}
		<p>There are no people to show...</p>
	{/each}
</main>
```

<br />

### ◼ Conditionals

```html
<script>
  let num = 15;
</script>

<main>
  {#if num > 20}
  <p>Greater than 20</p>
  {:else if num > 5}
  <p>Greater than 5</p>
  {:else}
  <p>Not Greater than 5</p>
  {/if}
</main>
```

<br />

### ◼ CSS & Conditional styles

- global.css에 작성한 스타일은 모든 component에 적용
- component안에서 작성한 스타일은 해당 component에서만 적용

```html
<script>
  let showModal = true;
  let isPromo = true;
</script>

{#if showModal}
<div class="backdrop" class:promo="{isPromo}">
  <div class="modal">
    <p>Sign up for offers!</p>
  </div>
</div>
{/if}
```

<br />

### ◼ Props

```html
// App.svelte
<Modal message="Hey, I am a prop value" isPromo="{true}" />

// Modal.svelte
<script>
  export let message = 'default value';
  export let isPromo = false;
</script>

{#if showModal}
<div class="backdrop" class:promo="{isPromo}">
  <div class="modal">
    <p>{message}</p>
  </div>
</div>
{/if}
```

<br />

### ◼ Event forwarding

- component를 사용한 곳에서 이벤트를 달아주고 싶을 때 사용 (해당 component에서는 이벤트를 달아주기만 하면 됨)

```html
// App.svelte
<Modal
  message="Hey, I am a prop value"
  isPromo="{true}"
  on:click="{toggleModal}"
/>

// Modal.svelte
<script>
  export let message = 'default value';
  export let isPromo = false;
</script>

{#if showModal}
<div class="backdrop" class:promo="{isPromo}" on:click>
  <div class="modal">
    <p>{message}</p>
  </div>
</div>
{/if}
```

<br />

### ◼ Event modifiers

- once: makes sure the event can only fire once (removes handler)
- preventDefault: prevent the default action (run e.preventDefault())
- self: only fires the event if the clicked element is the target

```html
{#if showModal}
<div class="backdrop" class:promo="{isPromo}" on:click|self>
  <div class="modal">
    <p>{message}</p>
  </div>
</div>
{/if}
```

<br />

### ◼ Slots

- react의 children과 같은 개념

```html
// App.svelte
<Modal
  message="Hey, I am a prop value"
  isPromo="{true}"
  on:click="{toggleModal}"
>
  <h3 slot="title">Add a New Person</h3>
  <form>
    <input type="text" placeholder="name" />
    <input type="text" placeholder="belt color" />
    <button>Add Person</button>
  </form>
  <Modal>
    // Modal.svelte
    <script>
      export let message = 'default value';
      export let isPromo = false;
    </script>

    {#if showModal}
    <div class="backdrop" class:promo="{isPromo}" on:click>
      <div class="modal">
        <slot name="title" />
        <slot />
      </div>
    </div>
    {/if}</Modal
  ></Modal
>
```

<br />

### ◼ Forms

```html
<script>
  let name;
  let beltColour;
  let age;
  // let fighting = false;
  // let sneaking = false;
  // let running = false;

  let skills = [];

  const handleSubmit = () => {
    console.log(name, beltColour, age, skills);
  };
</script>

<form on:submit|preventDefault="{handleSubmit}">
  <input type="text" placeholder="name" bind:value="{name}" />
  <input type="number" placeholder="age" bind:value="{age}" />
  <h3>Skills:</h3>
  <!--
  <input type="checkbox" bind:checked={fighting}>fighting<br>
  <input type="checkbox" bind:checked={sneaking}>sneaking<br>
  <input type="checkbox" bind:checked={running}>running<br>
  -->
  <input type="checkbox" bind:group="{skills}" value="fighting" />fighting<br />
  <input type="checkbox" bind:group="{skills}" value="sneaking" />sneaking<br />
  <input type="checkbox" bind:group="{skills}" value="running" />running<br />
  <h3>Belt Colour</h3>
  <select bind:value="{beltColour}">
    <option value="black">black</option>
    <option value="orange">orange</option>
    <option value="brown">brown</option>
    <option value="white">white</option>
  </select>
  <button>Add Person</button>
</form>
```

<br />

### ◼ Dispatching custom events

```html
// AddPersonForm.svelte
<script>
  import { createEventDispatcher } from 'svelte';

  let dispatch = createEventDispatcher();

  let name;
  let beltColour;
  let age;

  let skills = [];

  const handleSubmit = () => {
    const person = {
      name,
      beltColour,
      age,
      skills,
      id: Math.random(),
    };
    dispatch('addPerson', person);
  };
</script>

// App.svelte
<script>
  const addPerson = (e) => {
    people = [e.detail, ...people];
    toggleModal();
  };
</script>

<Modal message="Hey, I am a prop value" {showModal} on:click="{toggleModal}">
  <AddPersonForm on:addPerson="{addPerson}" />
</Modal>
```
