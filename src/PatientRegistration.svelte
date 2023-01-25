<script lang="ts">
  import { fhir } from "./fhir";
  import { onMount } from "svelte";
  import { navigate } from "svelte-routing";

  let loading: boolean = false;
  export let id;
  let form;

  let header: { "Content-Type": "application/json"; };

  console.log(id);
  async function handleSubmit(e: any) {
    loading = true;

    if (id){
      
      const r = await fhir.put(`/Patient/${id}`, { ...e.details, id }, { headers: header }).then((response) => {
        console.log(response.data);
      }).catch(e => {
        console.log(e)
      });
      //console.log(r.data);

    } else {
      const r = await fhir.post("/Patient", e.details, { headers: header }).then((response) => {
        console.log(response.data);
      }).catch(e => {
        console.log(e)
      });
      //console.log(r.data)
    }

    console.log(e.detail.name[1].given);
    console.log(e.detail);
    //console.log(r.data);
    loading = false;
    //navigate("/", { replace: true })
    }

  onMount(async () => {
    if (id){
      const r = await fhir.get(`/Patient/${id}`, { headers: header });
      const resource = r.data;
      form.import(resource);
	  }
  })

</script>


<h1 class="text-2xl font-semibold font-sans">Patient Registration</h1>
<mb-fhir-form
  bind:this={form}
  class="flex  pflex-col gap-3"
  on:mb-submit={handleSubmit}
>

<mb-input path="resourceType" label="Resource Type" />
<mb-select type="code" label="Status" path="active">
  <mb-option value="true" label="Active" />
  <mb-option value="false" label="Inactive" />
</mb-select>
<mb-select type="code" label="Type" path="name[0].use">
  <mb-option value="temp" label="Temp" />
  <mb-option value="old" label="Old" />
  <mb-option value="usual" label="Usual" />
  <mb-option value="official" label="Official" />
  <mb-option value="nickname" label="Name" />
  <mb-option value="anonymous" label="Anonymous" />
  <mb-option value="other" label="Other" />
</mb-select>
<mb-input path="name[3].prefix" label="Prefix" />
<mb-input path="name[1].given" label="First Name" />
<mb-input path="name[2].family" label="Last Name" />
<mb-input path="name[4].suffix" label="Suffix" />
<mb-date label="Date of Birth" path="birthDate" />
<mb-buttons type="code" label="Gender" path="gender" >
  <mb-option value="male" label="Male" />
  <mb-option value="female" label="Female" />
  <mb-option value="other" label="Other" />
</mb-buttons>
<mb-select type="code" path="telecom[0].system" label="Contact Type 1">
  <mb-option value="email" label="Email" />
  <mb-option value="phone" label="Phone" />
  <mb-option value="fax" label="Fax" />
  <mb-option value="pager" label="Pager" />
  <mb-option value="sms" label="SMS" />
  <mb-option value="other" label="Other" />
</mb-select>
<mb-select type="code" path="telecom[0].use" label="Use">
  <mb-option value="home" label="Home" />
  <mb-option value="work" label="Work" />
  <mb-option value="temp" label="Temp" />
  <mb-option value="old" label="Old" />
  <mb-option value="mobile" label="Mobile" />
</mb-select>
<mb-input path="telecom[0].value" label="Value" />
<mb-select type="code" path="telecom[1].system" label="Contact Type 2">
  <mb-option value="email" label="Email" />
  <mb-option value="phone" label="Phone" />
  <mb-option value="fax" label="Fax" />
  <mb-option value="pager" label="Pager" />
  <mb-option value="sms" label="SMS" />
  <mb-option value="other" label="Other" />
</mb-select>
<mb-select type="code" path="telecom[1].use" label="Use">
  <mb-option value="home" label="Home" />
  <mb-option value="work" label="Work" />
  <mb-option value="temp" label="Temp" />
  <mb-option value="old" label="Old" />
  <mb-option value="mobile" label="Mobile" />
</mb-select>
<mb-input path="telecom[1].value" label="Value" />
<mb-select type="code" path="address[0].type" label="Address Use" >
  <mb-option value="postal" label="Postal" />
  <mb-option value="physical" label="Physical" />
  <mb-option value="both" label="Both" />
</mb-select>
<mb-input path="address[0].line" label="Line" />
<mb-input path="address[0].city" label="City" />
<mb-input path="address[0].state" label="State" />
<mb-input path="address[0].postalCode" label="Postal Code" />
<mb-input path="address[0].country" label="Country" />
<mb-submit>
  <sl-button class="text-blue font-bold" {loading} type="info">Submit</sl-button>
</mb-submit>
</mb-fhir-form>
