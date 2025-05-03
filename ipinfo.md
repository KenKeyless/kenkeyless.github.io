---
layout: default
title: ""
permalink: /ipinfo/
---
<div id="ip-details">
</div>

<script>
  const PRIMARY_API = 'https://ipapi.co/json/';
  const FALLBACK_API = 'http://ip-api.com/json/?fields=continent,country,regionName,city,timezone,isp,org';

  // Fetch and parse JSON from API
  async function fetchJson(url) {
    try {
      const response = await fetch(url);
      return await response.ok ? response.json() : null;
    } catch (error) {
      console.error('Request failed:', error);
      return null;
    }
  }

  // Try primary API, fallback if needed
  async function getIpDetails() {
    let ipData = await fetchJson(PRIMARY_API);
    if (!ipData) {
      ipData = await fetchJson(FALLBACK_API);
    }
    return ipData;
  }

  // Generate HTML from IP data
  function createIpDetailsHtml(ipData) {
    if (!ipData) return '<p>Request failed</p>';

    return `
      <p><strong>ISP:</strong> ${ipData.org || ipData.isp || '-'}</p>
      <p><strong>IP Address:</strong> ${ipData.query || ipData.ip || '-'}</p>
      <p><strong>Country:</strong> ${ipData.country_name || ipData.country || '-'}</p>
      <p><strong>Timezone:</strong> ${ipData.timezone || '-'}</p>
      <p><strong>Region:</strong> ${ipData.regionName || ipData.region || '-'}</p>
      <p><strong>City:</strong> ${ipData.city || '-'}</p>
      <p><strong>Browser:</strong> ${navigator.userAgent}</p>
    `;
  }

  window.addEventListener('load', async () => {
    const ipDetailsContainer = document.getElementById('ip-details');
    const ipData = await getIpDetails();
    ipDetailsContainer.innerHTML = ipData ? createIpDetailsHtml(ipData) : '<p>Request failed</p>';
  });
</script>
