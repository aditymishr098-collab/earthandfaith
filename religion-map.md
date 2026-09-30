---
layout: page
title: World Religion Map
permalink: /religion-map/
---

Which religion has the most followers in each country? Tap or hover over a country to see it. The table below has the same information and can be searched.

<style>
#rm-legend { display: flex; flex-wrap: wrap; gap: 0.5rem 1.1rem; margin: 1rem 0; font-size: 0.85rem; }
#rm-legend span { display: inline-flex; align-items: center; gap: 0.4rem; }
#rm-legend i, .rm-dot { display: inline-block; width: 0.85rem; height: 0.85rem; border-radius: 50%; }
#rm-map { min-height: 160px; }
#rm-map path { stroke: rgba(128,128,128,0.45); stroke-width: 0.5; cursor: pointer; }
#rm-map path:hover { stroke: var(--ink, #000); stroke-width: 1.2; }
#rm-info { min-height: 1.8rem; margin: 0.6rem 0 1.4rem; font-size: 1rem; }
#rm-search { width: 100%; padding: 0.6rem 0.9rem; font: inherit; color: var(--ink, inherit); background: transparent; border: 1px solid var(--line, #ccc); border-radius: 8px; margin-bottom: 0.8rem; }
#rm-table { width: 100%; font-size: 0.9rem; }
#rm-table td { padding: 0.35rem 0.6rem; }
</style>
<div id="rm-legend"></div>
<div id="rm-map">Loading map...</div>
<div id="rm-info" role="status">Tap a country to see its largest religious group.</div>
<input id="rm-search" type="search" placeholder="Search a country..." aria-label="Search a country">
<table id="rm-table"><tbody></tbody></table>
<script src="https://cdnjs.cloudflare.com/ajax/libs/d3/7.8.5/d3.min.js"></script>
<script>
(function () {
  var RAW = "USA|United States|C;CAN|Canada|C;MEX|Mexico|C;GTM|Guatemala|C;BLZ|Belize|C;SLV|El Salvador|C;HND|Honduras|C;NIC|Nicaragua|C;CRI|Costa Rica|C;PAN|Panama|C;CUB|Cuba|C;JAM|Jamaica|C;HTI|Haiti|C;DOM|Dominican Republic|C;BHS|Bahamas|C;TTO|Trinidad and Tobago|C;COL|Colombia|C;VEN|Venezuela|C;ECU|Ecuador|C;PER|Peru|C;BOL|Bolivia|C;CHL|Chile|C;ARG|Argentina|C;URY|Uruguay|C;PRY|Paraguay|C;BRA|Brazil|C;GUY|Guyana|C;SUR|Suriname|C;PRI|Puerto Rico|C;GRL|Greenland|C;GBR|United Kingdom|C;IRL|Ireland|C;FRA|France|C;ESP|Spain|C;PRT|Portugal|C;ITA|Italy|C;DEU|Germany|C;NLD|Netherlands|C;BEL|Belgium|C;LUX|Luxembourg|C;CHE|Switzerland|C;AUT|Austria|C;POL|Poland|C;CZE|Czechia|N;SVK|Slovakia|C;HUN|Hungary|C;ROU|Romania|C;BGR|Bulgaria|C;SRB|Serbia|C;HRV|Croatia|C;SVN|Slovenia|C;BIH|Bosnia and Herzegovina|I;MNE|Montenegro|C;MKD|North Macedonia|C;ALB|Albania|I;KOS|Kosovo|I;GRC|Greece|C;CYP|Cyprus|C;MLT|Malta|C;ISL|Iceland|C;NOR|Norway|C;SWE|Sweden|C;DNK|Denmark|C;FIN|Finland|C;EST|Estonia|N;LVA|Latvia|C;LTU|Lithuania|C;BLR|Belarus|C;UKR|Ukraine|C;MDA|Moldova|C;RUS|Russia|C;GEO|Georgia|C;ARM|Armenia|C;AZE|Azerbaijan|I;TUR|Turkey|I;SAU|Saudi Arabia|I;YEM|Yemen|I;OMN|Oman|I;ARE|United Arab Emirates|I;QAT|Qatar|I;KWT|Kuwait|I;BHR|Bahrain|I;IRQ|Iraq|I;IRN|Iran|I;SYR|Syria|I;LBN|Lebanon|I;JOR|Jordan|I;PSE|Palestine|I;ISR|Israel|J;EGY|Egypt|I;LBY|Libya|I;TUN|Tunisia|I;DZA|Algeria|I;MAR|Morocco|I;ESH|Western Sahara|I;SDN|Sudan|I;MRT|Mauritania|I;MLI|Mali|I;NER|Niger|I;TCD|Chad|I;SEN|Senegal|I;GMB|Gambia|I;GIN|Guinea|I;GNB|Guinea-Bissau|I;SLE|Sierra Leone|I;BFA|Burkina Faso|I;CIV|Ivory Coast|S;LBR|Liberia|C;GHA|Ghana|C;TGO|Togo|C;BEN|Benin|C;NGA|Nigeria|S;CMR|Cameroon|C;GNQ|Equatorial Guinea|C;GAB|Gabon|C;COG|Republic of the Congo|C;COD|DR Congo|C;CAF|Central African Republic|C;AGO|Angola|C;ZMB|Zambia|C;MWI|Malawi|C;MOZ|Mozambique|C;ZWE|Zimbabwe|C;BWA|Botswana|C;NAM|Namibia|C;ZAF|South Africa|C;LSO|Lesotho|C;SWZ|Eswatini|C;MDG|Madagascar|C;TZA|Tanzania|C;KEN|Kenya|C;UGA|Uganda|C;RWA|Rwanda|C;BDI|Burundi|C;ETH|Ethiopia|C;ERI|Eritrea|C;SSD|South Sudan|C;DJI|Djibouti|I;SOM|Somalia|I;COM|Comoros|I;MUS|Mauritius|H;IND|India|H;PAK|Pakistan|I;BGD|Bangladesh|I;LKA|Sri Lanka|B;NPL|Nepal|H;BTN|Bhutan|B;MDV|Maldives|I;AFG|Afghanistan|I;UZB|Uzbekistan|I;TKM|Turkmenistan|I;TJK|Tajikistan|I;KGZ|Kyrgyzstan|I;KAZ|Kazakhstan|I;MNG|Mongolia|B;CHN|China|N;PRK|North Korea|N;KOR|South Korea|N;JPN|Japan|N;TWN|Taiwan|F;MMR|Myanmar|B;THA|Thailand|B;KHM|Cambodia|B;LAO|Laos|B;VNM|Vietnam|F;MYS|Malaysia|I;IDN|Indonesia|I;BRN|Brunei|I;SGP|Singapore|B;PHL|Philippines|C;TLS|Timor-Leste|C;AUS|Australia|C;NZL|New Zealand|C;PNG|Papua New Guinea|C;FJI|Fiji|C;SLB|Solomon Islands|C;VUT|Vanuatu|C;NCL|New Caledonia|C";
  var G = {C:["Christianity","#3b6fb6"],I:["Islam","#2e9e6b"],H:["Hinduism","#e3762f"],B:["Buddhism","#d9a520"],J:["Judaism","#8d5bd0"],F:["Folk religions","#b0673c"],N:["No religion","#8b969e"],S:["No clear majority","#d1578f"]};
  var rows = RAW.split(";").map(function (s) { var p = s.split("|"); return {id: p[0], name: p[1], g: p[2]}; });
  var byId = {}, counts = {};
  rows.forEach(function (r) { byId[r.id] = r; counts[r.g] = (counts[r.g] || 0) + 1; });
  var info = document.getElementById("rm-info");
  function show(r, fallbackName) {
    info.textContent = r ? r.name + ": " + G[r.g][0] : (fallbackName || "") + ": no data";
  }
  document.getElementById("rm-legend").innerHTML = Object.keys(G).map(function (k) {
    return '<span><i style="background:' + G[k][1] + '"></i>' + G[k][0] + ' (' + (counts[k] || 0) + ')</span>';
  }).join("");
  var tbody = document.querySelector("#rm-table tbody");
  var sorted = rows.slice().sort(function (a, b) { return a.name.localeCompare(b.name); });
  function renderTable(q) {
    q = (q || "").toLowerCase();
    tbody.innerHTML = sorted.filter(function (r) { return r.name.toLowerCase().indexOf(q) > -1 || G[r.g][0].toLowerCase().indexOf(q) > -1; }).map(function (r) {
      return '<tr><td>' + r.name + '</td><td><span class="rm-dot" style="background:' + G[r.g][1] + '"></span> ' + G[r.g][0] + '</td></tr>';
    }).join("");
  }
  renderTable("");
  document.getElementById("rm-search").addEventListener("input", function (e) { renderTable(e.target.value); });
  var mapBox = document.getElementById("rm-map");
  if (!window.d3) { mapBox.textContent = "The map could not be loaded. The table below has the same information."; return; }
  d3.json("https://cdn.jsdelivr.net/gh/johan/world.geo.json@master/countries.geo.json").then(function (geo) {
    geo.features = geo.features.filter(function (f) { return f.id !== "ATA"; });
    var w = 960, h = 500;
    var path = d3.geoPath(d3.geoNaturalEarth1().fitSize([w, h], geo));
    mapBox.innerHTML = "";
    var svg = d3.select(mapBox).append("svg").attr("viewBox", "0 0 " + w + " " + h).attr("role", "img").attr("aria-label", "World map coloured by largest religious group").style("width", "100%").style("height", "auto");
    svg.selectAll("path").data(geo.features).join("path").attr("d", path)
      .attr("fill", function (d) { var r = byId[d.id]; return r ? G[r.g][1] : "#c9ced2"; })
      .on("mouseover", function (e, d) { show(byId[d.id], d.properties.name); })
      .on("click", function (e, d) { show(byId[d.id], d.properties.name); });
  }).catch(function () {
    mapBox.textContent = "The map could not be loaded. The table below has the same information.";
  });
})();
</script>

## About this map

The colours show the **largest religious group** in each country, based mainly on estimates from the Pew Research Center (roughly 2010 to 2020). Please read it as a simple overview, not as exact data:

- "No religion" means the largest group is people with no religious affiliation, as in China and Japan.
- "No clear majority" is used when two groups are almost equal.
- Some countries are simplified. Many have several large groups, and numbers change over time.
- "Folk religions" follows Pew's classification, for example for Taiwan and Vietnam.
- Some small countries and islands may not appear on the map, but they are included in the table when I have data for them.

If you spot a mistake, please tell me through the [Contact](/contact/) page. You can also see my [timeline of religions](/timeline/) and the post on [all the religions of the world](/2026/09/29/all-religions-of-the-world.html).
