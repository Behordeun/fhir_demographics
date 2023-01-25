<script lang="ts">
  import "medblocks-ui";
  import "@shoelace-style/shoelace/dist/themes/base.css";
  import "./tailwind.css";
  import { Router, Link, Route} from "svelte-routing";
  import PatientRegistration from "./PatientRegistration.svelte";
  import Layout from "./Layout.svelte";
  import List from "./List.svelte";
</script>

<Router>
  <Layout>
    <Route path="/" component="{List}" />
    <Route path="patient"  component="{ PatientRegistration }" />
    <Route path="patient/:id"  component="{ PatientRegistration }" />
  </Layout>
</Router>