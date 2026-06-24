---
title: "CSO Spillcast"
permalink: /cso_spillcast/
author_profile: false
---

{% include base_path %}

**CSO Spillcast** is an experimental tool that forecasts the probability of
combined sewer overflow (CSO) spills over the next 48 hours, using a machine
learning model driven by rainfall forecasts and catchment wetness. Forecasts are
refreshed every 6 hours; **all times are UTC**.

Pick a CSO below to see its live forecast, a reliability check on recent
unseen data, and a downloadable forecast table.

<table>
  <thead>
    <tr>
      <th>Permit</th>
      <th>Location</th>
      <th>Last updated (UTC)</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
  {% for cso in site.data.cso_spillcast %}
    <tr>
      <td><code>{{ cso.permit }}</code></td>
      <td>{{ cso.name }}</td>
      <td><span class="spillcast-updated" data-permit="{{ cso.permit }}">&mdash;</span></td>
      <td><a href="/cso_spillcast/{{ cso.slug }}/">view &rarr;</a></td>
    </tr>
  {% endfor %}
  </tbody>
</table>

<script>
  // Fill the "Last updated" column from the live manifest. This is the only
  // piece that needs the S3 bucket's CORS rule (GET from alexlipp.github.io).
  (function () {
    var url = "{{ site.spillcast_manifest_url }}";
    if (!url) { return; }
    fetch(url, { cache: "no-store" })
      .then(function (r) { return r.json(); })
      .then(function (m) {
        (m.csos || []).forEach(function (c) {
          var sel = '.spillcast-updated[data-permit="' + c.permit + '"]';
          document.querySelectorAll(sel).forEach(function (el) {
            el.textContent = c.generated_at ? c.generated_at.replace("T", " ") : "—";
          });
        });
      })
      .catch(function () { /* leave dashes if the manifest can't be reached */ });
  })();
</script>

<p style="margin-top:1.5em;font-size:smaller;color:#666;">
  Experimental research output &mdash; not an official water-company product.
  Built on the <a href="https://github.com/AlexLipp/POOPy">POOPy</a> /
  <a href="https://www.sewagemap.co.uk/">SewageMap</a> ecosystem.
</p>
