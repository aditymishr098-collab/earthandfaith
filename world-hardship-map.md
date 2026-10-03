---
layout: page
title: "World Map: Hunger, Poverty, Conflict and Violence"
permalink: /world-hardship-map/
---

Where in the world do people face the most hunger, poverty, war and violence? Choose a topic below to colour the map. Darker colours mean a higher value. Tap a country to see its number. The data is loaded live from the World Bank's open data, so it updates when new figures are published.

<style>
#hm-ind, #hm-search { width: 100%; padding: 0.6rem 0.9rem; font: inherit; color: var(--ink, inherit); background: transparent; border: 1px solid var(--line, #ccc); border-radius: 8px; box-sizing: border-box; }
#hm-ind option { color: #17211f; background: #fff; }
#hm-unit { font-size: 0.9rem; color: var(--muted, #5d6b66); margin: 0.5rem 0 1rem; }
#hm-legend { display: flex; flex-wrap: wrap; gap: 0.4rem 1rem; margin: 0.6rem 0; font-size: 0.82rem; }
#hm-legend span { display: inline-flex; align-items: center; gap: 0.4rem; }
#hm-legend i { display: inline-block; width: 0.9rem; height: 0.9rem; border-radius: 3px; }
#hm-map { min-height: 160px; }
#hm-map path { stroke: rgba(128,128,128,0.45); stroke-width: 0.5; cursor: pointer; }
#hm-map path:hover { stroke: var(--ink, #000); stroke-width: 1.2; }
#hm-info { min-height: 1.8rem; margin: 0.6rem 0 1rem; }
#hm-wrap { max-height: 360px; overflow-y: auto; margin-top: 0.8rem; border: 1px solid var(--line, #ccc); border-radius: 8px; }
#hm-table { width: 100%; margin: 0; font-size: 0.9rem; border: 0; }
#hm-table td { padding: 0.35rem 0.7rem; }
#hm-status { font-size: 0.9rem; color: var(--muted, #5d6b66); }
</style>
<select id="hm-ind" aria-label="Choose a topic">
  <option value="hun">Hunger</option>
  <option value="pov">Extreme poverty</option>
  <option value="con">Violent conflict</option>
  <option value="hom">Killings (homicide)</option>
  <option value="vaw">Violence against women</option>
</select>
<p id="hm-unit"></p>
<div id="hm-legend"></div>
<div id="hm-map">Loading map...</div>
<div id="hm-info" role="status">Tap a country to see its value.</div>
<p id="hm-status"></p>
<input id="hm-search" type="search" placeholder="Search a country..." aria-label="Search a country">
<div id="hm-wrap"><table id="hm-table"><tbody></tbody></table></div>
<script src="https://cdnjs.cloudflare.com/ajax/libs/d3/7.8.5/d3.min.js"></script>
<script>
(function () {
  var IND = {
    hun: {code: "SN.ITK.DEFC.ZS", unit: "Hunger: the share of people who do not get enough food to live a healthy life (undernourishment, %).", fmt: function (v) { return v.toFixed(1) + "%"; }},
    pov: {code: "SI.POV.DDAY", unit: "Extreme poverty: the share of people living on less than $3.00 a day (adjusted for local prices, %).", fmt: function (v) { return v.toFixed(1) + "%"; }},
    con: {code: "VC.BTL.DETH", unit: "Violent conflict: people killed in battle in the latest year with data. This includes wars between countries as well as civil wars.", fmt: function (v) { return Math.round(v).toLocaleString("en-US") + " deaths"; }},
    hom: {code: "VC.IHR.PSRC.P5", unit: "Killings: intentional homicides per 100,000 people.", fmt: function (v) { return v.toFixed(1) + " per 100,000"; }},
    vaw: {code: "SG.VAW.1549.ZS", unit: "Violence against women: the share of women aged 15 to 49 who have had a partner and suffered physical or sexual violence from a partner in the past 12 months (%). Based on surveys, available for about 100 countries.", fmt: function (v) { return v.toFixed(1) + "%"; }}
  };
  var COLORS = ["#fde3bf", "#fbb97c", "#f3834f", "#d1493a", "#8b1c2b"];
  var names = null, cache = {}, cur = "hun", data = {}, paths = null;
  var $ = function (id) { return document.getElementById(id); };

  function getJSON(url) {
    return fetch(url).then(function (r) { if (!r.ok) { throw new Error(r.status); } return r.json(); });
  }
  function loadNames() {
    if (names) { return Promise.resolve(names); }
    return getJSON("https://api.worldbank.org/v2/country?format=json&per_page=400").then(function (j) {
      var s = {};
      (j[1] || []).forEach(function (c) { if (c.region && c.region.value !== "Aggregates") { s[c.id] = c.name; } });
      names = s;
      return s;
    });
  }
  function loadInd(key) {
    if (cache[key]) { return Promise.resolve(cache[key]); }
    var url = "https://api.worldbank.org/v2/country/all/indicator/" + IND[key].code + "?format=json&mrnev=1&per_page=500";
    return Promise.all([loadNames(), getJSON(url)]).then(function (res) {
      var out = {};
      (res[1][1] || []).forEach(function (r) {
        var id = r.countryiso3code;
        if (id && res[0][id] && r.value !== null) { out[id] = {v: r.value, y: r.date, n: res[0][id]}; }
      });
      cache[key] = out;
      return out;
    });
  }
  function legend(sc) {
    var f = IND[cur].fmt, q = sc.quantiles(), html = "";
    var labels = ["Lowest fifth", "", "", "", "Highest fifth"];
    for (var i = 0; i < COLORS.length; i++) {
      var range = i === 0 ? "up to " + f(q[0]) : i === 4 ? "from " + f(q[3]) : f(q[i - 1]) + " to " + f(q[i]);
      html += '<span><i style="background:' + COLORS[i] + '"></i>' + (labels[i] ? labels[i] + ": " : "") + range + '</span>';
    }
    html += '<span><i style="background:#c9ced2"></i>No data</span>';
    $("hm-legend").innerHTML = html;
  }
  function table() {
    var q = $("hm-search").value.toLowerCase(), f = IND[cur].fmt;
    var rows = Object.keys(data).map(function (k) { return data[k]; }).sort(function (a, b) { return b.v - a.v; })
      .filter(function (r) { return r.n.toLowerCase().indexOf(q) > -1; });
    $("hm-table").querySelector("tbody").innerHTML = rows.map(function (r) {
      return "<tr><td>" + r.n + "</td><td>" + f(r.v) + "</td><td>" + r.y + "</td></tr>";
    }).join("");
  }
  function show(id, nm) {
    var r = data[id];
    $("hm-info").textContent = r ? r.n + ": " + IND[cur].fmt(r.v) + " (" + r.y + ")" : (nm || "") + ": no data";
  }
  function paint() {
    var vals = Object.keys(data).map(function (k) { return data[k].v; });
    var sc = d3.scaleQuantile().domain(vals).range(COLORS);
    legend(sc);
    if (paths) { paths.attr("fill", function (d) { var r = data[d.id]; return r ? sc(r.v) : "#c9ced2"; }); }
    table();
  }
  function choose(key) {
    cur = key;
    $("hm-unit").textContent = IND[key].unit;
    $("hm-status").textContent = "Loading data...";
    loadInd(key).then(function (d) {
      if (cur !== key) { return; }
      data = d;
      $("hm-status").textContent = Object.keys(d).length + " countries have data. Each country shows its latest available year, which can differ.";
      if (window.d3) { paint(); }
    }).catch(function () {
      $("hm-status").textContent = "The data could not be loaded right now. Please try again later.";
    });
  }
  $("hm-ind").addEventListener("change", function (e) { choose(e.target.value); });
  $("hm-search").addEventListener("input", table);
  if (!window.d3) { $("hm-map").textContent = "The map library could not be loaded."; return; }
  choose("hun");
  d3.json("https://cdn.jsdelivr.net/gh/johan/world.geo.json@master/countries.geo.json").then(function (g) {
    g.features = g.features.filter(function (f) { return f.id !== "ATA"; });
    var w = 960, h = 500, box = $("hm-map");
    var path = d3.geoPath(d3.geoNaturalEarth1().fitSize([w, h], g));
    box.innerHTML = "";
    var svg = d3.select(box).append("svg").attr("viewBox", "0 0 " + w + " " + h).attr("role", "img").attr("aria-label", "World map coloured by the selected topic").style("width", "100%").style("height", "auto");
    paths = svg.selectAll("path").data(g.features).join("path").attr("d", path).attr("fill", "#c9ced2")
      .on("mouseover", function (e, d) { show(d.id, d.properties.name); })
      .on("click", function (e, d) { show(d.id, d.properties.name); });
    if (Object.keys(data).length) { paint(); }
  }).catch(function () {
    $("hm-map").textContent = "The map could not be loaded. The table below has the same information.";
  });
})();
</script>

## How to read this map

- **Darker means higher.** The colours split the countries into five equal groups. They show how countries compare with each other, not whether a number is "safe".
- **Grey means no data.** It does not mean there is no problem. Many countries, especially those in conflict, have gaps.
- **Years differ.** Each country shows its most recent figure, which may be several years old.
- **Conflict.** The numbers count deaths in battle recorded by the Uppsala Conflict Data Program. They include wars between countries, such as the war in Ukraine, and not only civil wars. They do not show civilians killed outside battles.
- **Killings.** Police and health records differ between countries, so small differences should not be over-read.
- **Violence against women.** This is a survey of violence by a partner. It is not a count of rape. Numbers of rapes reported to police cannot be compared between countries, because laws, trust in the police and willingness to report differ widely, and a higher number can even mean that victims are more able to speak up.
- **Discrimination.** There is no single reliable worldwide number for discrimination, so I have not shown one. Indicators such as the legal rights of women may be added later.

## A few things to keep in mind

These numbers describe where people are suffering. They do not show that the people, or the religions, of any country are to blame. The causes are many and often overlap: war, weak institutions, debt, climate, history and colonial rule. Comparing this map with the [World Religion Map](/religion-map/) can be tempting, but two maps that look similar do not prove that one causes the other.

*Data: World Bank Open Data (CC BY 4.0). The underlying sources differ by topic and include the Food and Agriculture Organization (hunger), the World Bank's Poverty and Inequality Platform (poverty), the Uppsala Conflict Data Program (conflict), the UN Office on Drugs and Crime (homicide) and national surveys (violence against women). Please check those organisations for exact definitions and the latest figures.*
