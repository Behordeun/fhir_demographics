<script lang="ts">
    import { fhir } from "./fhir";
    let loading: boolean = false;
    
  
    async function handleSubmit(e: any) {
        loading = true;
        export let id;
        let form;
        console.log(id)
  
        //const options = {
        //    url: "http://localhost:8090/fhir/Patient",
        //    content_type: "application/json",
        //    method: "POST",
        //    headers: {
        //      'X-requested-with':'XMLHttpRequest'
        //    },
      //};
  
  
      //await fhir.post("/Patient", data).then((response) => {
      //  console.log(response.data);
      //}).catch(e => {
      //  console.log(e);
      //});;

      //console.log("result", e.detail);
      ////console.log(e.data);
      //loading = false

        console.log(e);
        console.log(e.detail.name[0]);
        
        var myHeaders = new Headers();
        myHeaders.append("Content-Type", "application/json");
            
        var raw = JSON.stringify({
          "resourceType": e.detail.resourceType,
          "active": true,
          "name": [
            {
              "use": e.detail.name[0].code,
              "given": [
                e.detail.name[2].given
              ],
              "family": e.detail.name[3].family,
              "prefix": e.detail.name[1].prefix,
              "suffix": e.detail.name[4].code
            }
          ],
          "gender": e.detail.gender.code,
          "birthDate": e.detail.birthDate,
          "telecom": [
            {
              "value": e.detail.telecom[0].value,
              "use": e.detail.telecom[0].use.code,
              "system": e.telecom[0].system.code
            },
            {
              "value": e.detail.telecom[1].value,
              "use": e.detail.telecom[1].use.code,
              "system": e.detail.telecom[1].system
            }
          ],
          "address": [
            {
              "line": e.detail.address[0].line,
              "city": e.detail.address[0].city,
              "state": e.detail.address[0].state,
              "country": e.detail.address[0].country
            }
          ]
        });

        var requestOptions = {
          method: 'POST',
          headers: myHeaders,
          body: raw,
          redirect: 'follow'
        };

        fetch("http://localhost:8090/fhir/Patient/", requestOptions)
          .then(response => response.text())
          .then(result => console.log(result))
          .catch(error => console.log('error', error));
        }
    
</script>
  
  
<h1 class="text-2xl font-semibold font-sans">Patient Registration</h1>
<mb-fhir-form class="flex  pflex-col gap-3" on:mb-submit={handleSubmit}>
<mb-select type="code" label="Type" path="name[0].use">
    <mb-option value="temp" label="Temp" />
    <mb-option value="old" label="Old" />
    <mb-option value="usual" label="Usual" />
    <mb-option value="official" label="Official" />
    <mb-option value="nickname" label="Name" />
    <mb-option value="annonymous" label="Annonymous" />
    <mb-option value="other" label="Other" />
</mb-select>
<mb-input path="name[1].prefix" label="Prefix" />
<mb-input path="name[2].given" label="First Name" />
<mb-input path="name[3].family" label="Last Name" />
<mb-input path="name[4].suffix" label="Suffix" />
<mb-date label="Date of Birth" path="birthDate" />
<mb-buttons type="code" label="Gender" path="gender" >
    <mb-option value="male" label="Male" />
    <mb-option value="female" label="Female" />
    <mb-option value="other" label="Other" />
</mb-buttons>
<mb-select path="telecom[0].use" label="Phone Use">
    <mb-option value="home" label="Home" />
    <mb-option value="work" label="Work" />
    <mb-option value="temp" label="Temp" />
    <mb-option value="old" label="Old" />
    <mb-option value="mobile" label="Mobile" />
</mb-select>
<mb-input path="telecom[0].value" label="Phone Number" />
<mb-select path="telecom[1].use" label="Email Use">
    <mb-option value="home" label="Home" />
    <mb-option value="work" label="Work" />
    <mb-option value="temp" label="Temp" />
    <mb-option value="old" label="Old" />
    <mb-option value="mobile" label="Mobile" />
</mb-select>
<mb-input path="telecom[1].value" label="Email Address" />
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
    <sl-button class="text-blue-600 font-bold" {loading} type="info">Submit</sl-button>
</mb-submit>
</mb-fhir-form>
  