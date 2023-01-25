<script lang="ts">
    import { onMount } from "svelte";
    import { fhir } from "./fhir";
    import { Link } from "svelte-routing";

    let data = [];
    onMount(async () => {
        const r = await fhir.get("/Patient")
        console.log(r)
        data = r.data?.entry;
    });
</script>

<h1 class="text-4xl">Patients</h1>
<Link to="patient">
    <sl-button type="primary">New Patient</sl-button>
</Link>
<a href="/patient" rel="noopener noreferrer" target="_blank">
    <button>New Patient</button>
</a>
<div>
    {#each data as patient}
    <p>
    <Link to={`patient/${patient.resource.id}`} class="text-blue font-bold"
    >{patient.resource.name[0].given}</Link>
    </p>
    {/each}
</div>