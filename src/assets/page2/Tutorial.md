##  How to use Huaweicloud 

### Setup

The following is the Huawei Cloud server setup process and subsequent usage.


First, go to the Huawei Cloud official website, choose Elastic Cloud Server (ECS).

![container type](/presentation/src/assets/page2/images/containerType.png)

The second step is to start setting up the server.Since it was required for the course, we chose the cheaper usage plan.

![bullseye](/presentation/src/assets/page2/images/bullseye.png)

And select CentOSf for this experiment

![no options](/presentation/src/assets/page2/images/noOptions.png)
Next, configure the network.Use Huawei Cloud's default IP address

```json
	"devDependencies": {
		"@sveltejs/adapter-auto": "^2.0.0",
		"@sveltejs/kit": "^1.20.4",
		"prettier": "^2.8.0",
		"prettier-plugin-svelte": "^2.10.1",
		"svelte": "^4.2.1",
		"svelte-check": "^3.4.3",
		"tslib": "^2.4.1",
		"typescript": "^5.0.0",
		"vite": "^4.4.2"
	},
```

Sveltestrap is not fully updated to Svelte version 4 so as a work around cd .. back to the folder SvelteTS23 and add add a package.json file.

```json
{
    "dependencies": {
        "svelte": "^3.59.2",
        "sveltestrap": "^5.11.2",
        "vite":"4.4.11"
    }
}    
```

> npm install

Now change directory back to my-app.

>cd my-app

> npm install --save sveltestrap svelte

Finally install the axios library to retrieve json data from a rest API.

> npm i axios@1.5.1

### Svelte code

Check the contents of app.html in the src folder

app.html
```html
<!DOCTYPE html>
<html lang="en">
	<head>
		<meta charset="utf-8" />
		<link rel="icon" href="%sveltekit.assets%/favicon.png" />
		<meta name="viewport" content="width=device-width, initial-scale=1" />
		%sveltekit.head%
	</head>
	<body data-sveltekit-preload-data="hover">
		<div style="display: contents">%sveltekit.body%</div>
	</body>
</html>
```

Then start to edit +page.svelte in the routes folder.

```javascript
<svelte:head>
  <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css">
</svelte:head>
```
Sveltestrap is only a wrapper for bootstrap and does not include the bootstrap CSS.  This can be included in a number of different ways described on the sveltestrap site, but until versioning is completely compatible with Svelte 4 this is a reliable way to bring the css in from a contend delivery network.

This code will insert into the head of the html file.

```javascript          

<script lang="ts">
  import { onMount } from "svelte";
  import axios from "axios";
  import { Card,
    CardBody,
    CardFooter,
    CardHeader,
    CardSubtitle,
    CardText,
    CardTitle } from "sveltestrap"; 
```
The lang="ts" identifies this script as typescript.

The import of onMount allows svelte to perform programmed functions when the page loads.

Axios is the library to retrieve JSON from the rest API.  This can be used as an alternative to fetch as discussed by [David Adeneye](https://www.sitepoint.com/svelte-fetch-data/).

The Card elements are imports from the sveltstrap library.  [The components available are listed](https://sveltestrap.js.org/v4/?path=/story/components--get-started).



```javascript

const endpoint = "https://jsonplaceholder.typicode.com/users";

interface Company{
  name:string
  catchPhrase:string;
  bs:string;
}

interface User {
  id: number;
  name: string;
  email: string;
  company:Company;
}

let values: User[] = [];
```
The endpoint is defined as a convenience.

For typescript the array of values must have a type of structure which matches the JSON data so interfaces are set up to match the data.


```javascript
onMount(async function () {
  const response = await axios.get(endpoint);
  console.log(response.data);
  values = response.data;
});

</script>
```
The asynchronous function retrieves data from the endpoint and stores response data into the values array.

```javascript

{#each values as item}               
<Card color="success">
  <CardHeader>
    <CardTitle>{item.name}</CardTitle>
  </CardHeader>
  <CardBody>
    <CardSubtitle>{item.email}</CardSubtitle>
    <CardText>
      {item.company.name}
    </CardText>
  </CardBody>
  <CardFooter>    
      {item.company.catchPhrase}
  </CardFooter>
</Card>
<br/>
{/each}
```

The #each ... /each syntax iterates around the values array storing one array element as item on each iteration.

item has the sturucture of a User so its company fields can be accessed and information is rendered on screen.

A color is added to the card, you can experiment with the styling of the card.

The full listing is 
**my-app/presentation/src/routes/+page.svelte**
```javascript
<svelte:head>
  <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css">
</svelte:head>
          

<script lang="ts">
  import { onMount } from "svelte";
  import axios from "axios";
  import { Card,
    CardBody,
    CardFooter,
    CardHeader,
    CardSubtitle,
    CardText,
    CardTitle } from "sveltestrap"; 


const endpoint = "https://jsonplaceholder.typicode.com/users";

interface Company{
  name:string
  catchPhrase:string;
  bs:string;
}

interface User {
  id: number;
  name: string;
  email: string;
  company:Company;
}

let values: User[] = [];

onMount(async function () {
  const response = await axios.get(endpoint);
  console.log(response.data);
  values = response.data;
});

</script>

{#each values as item}               
<Card color="success">
  <CardHeader>
    <CardTitle>{item.name}</CardTitle>
  </CardHeader>
  <CardBody>
    <CardSubtitle>{item.email}</CardSubtitle>
    <CardText>
      {item.company.name}
    </CardText>
  </CardBody>
  <CardFooter>    
      {item.company.catchPhrase}
  </CardFooter>
</Card>
<br/>
{/each}


<card >
	{#each values as item}
                  <option value={item.id}>{item.name}</option>
                {/each}
</card>
```

The working output is then:

![user list](/presentation/src/assets/page2/images/userList.png)

The Svelte code is pretty straight forward, the biggest difficulty was the versioning changes around the style sheet code, that should be fixed in time.