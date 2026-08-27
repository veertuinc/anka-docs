---
_build:
  render: never
  list: never
---
{{< rawhtml >}}
<style>
.vm-version-info-btn {
  cursor: pointer;
  display: inline-flex;
  align-items: center;
  justify-content: center;
  width: 1.4em;
  height: 1.4em;
  margin-left: 6px;
  padding: 0;
  background: #f0f0f0;
  border: none;
  font-size: 1em;
  color: #555;
  transition: background 0.2s, color 0.2s;
}
.vm-version-info-btn:hover {
  background: #e0e0e0;
  color: #333;
}
.vm-version-notes {
  text-align: left;
  font-size: 0.9em;
  display: none;
  margin-top: 8px;
}
.vm-version-notes.is-open {
  display: block;
}
.vm-version-notes ul {
  margin: 0 0 0 1em;
  padding: 0;
}
.vm-version-notes li {
  margin-bottom: 0.25em;
}
.vm-version-notes li:last-child {
  margin-bottom: 0;
}
.vm-version-notes pre {
  margin: 8px 0 0;
  max-width: 100%;
  overflow-x: auto;
  white-space: pre-wrap;
  word-break: break-word;
}
.vm-version-tables {
  display: flex;
  text-align: center;
}
.vm-version-tables > div {
  width: 50%;
  min-width: 0;
}
.vm-version-tables table {
  width: 100%;
  table-layout: fixed;
}
.vm-version-tables tr td:nth-child(2) {
  width: 3em;
}
</style>
<script>
document.addEventListener('DOMContentLoaded', function() {
  document.querySelectorAll('.vm-version-info-btn').forEach(function(btn) {
    btn.addEventListener('click', function() {
      var notes = this.closest('td').querySelector('.vm-version-notes');
      if (notes) notes.classList.toggle('is-open');
    });
  });
});
</script>
<div class="vm-version-tables">
<div>
<h4 style="padding: 10px;">Anka 3 (arm64/Silicon)</h4>
<table>
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/4.7.0/css/font-awesome.min.css">
<tbody style="text-align:center;">
  <tr>
    <td style="vertical-align: middle">
      <b>
        macOS 26.6.2 (25G83)
        <a href="https://updates.cdn-apple.com/2026SummerFCS/fullrestores/140-75212/A2A24B94-1FC1-45A3-93F7-C51B02AF1F4D/UniversalMac_26.6.2_25G83_Restore.ipsw" title="Download IPSW" style="margin-left: 8px; text-decoration: none;" target="_blank" rel="noopener">
          <span class="fa fa-download" style="font-size:1.2em;"></span>
        </a>
        <button type="button" class="vm-version-info-btn" aria-label="Version requirements">&#8505;</button>
      </b>
      <blockquote class="hint info vm-version-notes is-open"><ul><li>Released 2026-08-17.</li><li>Requires setting a disk size of 50GB or more.</li></ul></blockquote>
    </td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle">
      <b>
        macOS 26.6.1 (25G76)
        <a href="https://updates.cdn-apple.com/2026SummerFCS/fullrestores/140-83079/25315EF6-AEAB-4588-9774-A3723774C47F/UniversalMac_26.6.1_25G76_Restore.ipsw" title="Download IPSW" style="margin-left: 8px; text-decoration: none;" target="_blank" rel="noopener">
          <span class="fa fa-download" style="font-size:1.2em;"></span>
        </a>
        <button type="button" class="vm-version-info-btn" aria-label="Version requirements">&#8505;</button>
      </b>
      <blockquote class="hint info vm-version-notes"><ul><li>Released 2026-08-06.</li><li>Requires <a href="https://downloads.veertu.com/anka/MobileDevice-26.4.pkg" target="_blank" rel="noopener">MobileDevice-26.4.pkg</a> to be installed on the host.</li><li>Requires setting a disk size of 50GB or more.</li></ul></blockquote>
    </td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle">
      <b>
        macOS 26.6 (25G72)
        <a href="https://updates.cdn-apple.com/2026SummerFCS/fullrestores/140-65618/10445B26-DE2C-43EC-9149-0A831602E74B/UniversalMac_26.6_25G72_Restore.ipsw" title="Download IPSW" style="margin-left: 8px; text-decoration: none;" target="_blank" rel="noopener">
          <span class="fa fa-download" style="font-size:1.2em;"></span>
        </a>
        <button type="button" class="vm-version-info-btn" aria-label="Version requirements">&#8505;</button>
      </b>
      <blockquote class="hint info vm-version-notes"><ul><li>Released 2026-07-27.</li><li>Requires <a href="https://downloads.veertu.com/anka/MobileDevice-26.4.pkg" target="_blank" rel="noopener">MobileDevice-26.4.pkg</a> to be installed on the host.</li><li>Requires setting a disk size of 50GB or more.</li></ul></blockquote>
    </td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle">
      <b>
        macOS 26.5.2 (25F84)
        <a href="https://updates.cdn-apple.com/2026SpringFCS/fullrestores/140-24263/B95838F0-6815-4F0B-A039-156526C081AD/UniversalMac_26.5.2_25F84_Restore.ipsw" title="Download IPSW" style="margin-left: 8px; text-decoration: none;" target="_blank" rel="noopener">
          <span class="fa fa-download" style="font-size:1.2em;"></span>
        </a>
        <button type="button" class="vm-version-info-btn" aria-label="Version requirements">&#8505;</button>
      </b>
      <blockquote class="hint info vm-version-notes"><ul><li>Released 2026-06-29.</li><li>Requires <a href="https://downloads.veertu.com/anka/MobileDevice-26.4.pkg" target="_blank" rel="noopener">MobileDevice-26.4.pkg</a> to be installed on the host.</li><li>Requires setting a disk size of 50GB or more.</li></ul></blockquote>
    </td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle">
      <b>
        macOS 26.5.1 (25F80)
        <a href="https://updates.cdn-apple.com/2026SpringFCS/fullrestores/122-88870/E47EBB85-45F2-4E3C-B9E7-6FF7868C2FBA/UniversalMac_26.5.1_25F80_Restore.ipsw" title="Download IPSW" style="margin-left: 8px; text-decoration: none;" target="_blank" rel="noopener">
          <span class="fa fa-download" style="font-size:1.2em;"></span>
        </a>
        <button type="button" class="vm-version-info-btn" aria-label="Version requirements">&#8505;</button>
      </b>
      <blockquote class="hint info vm-version-notes"><ul><li>Released 2026-06-02.</li><li>Requires <a href="https://downloads.veertu.com/anka/MobileDevice-26.4.pkg" target="_blank" rel="noopener">MobileDevice-26.4.pkg</a> to be installed on the host.</li><li>Requires setting a disk size of 50GB or more.</li></ul></blockquote>
    </td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle">
      <b>
        macOS 26.5 (25F71)
        <a href="https://updates.cdn-apple.com/2026SpringFCS/fullrestores/122-58869/DFB1CEEF-5619-4591-9924-E20DB2C8FED0/UniversalMac_26.5_25F71_Restore.ipsw" title="Download IPSW" style="margin-left: 8px; text-decoration: none;" target="_blank" rel="noopener">
          <span class="fa fa-download" style="font-size:1.2em;"></span>
        </a>
        <button type="button" class="vm-version-info-btn" aria-label="Version requirements">&#8505;</button>
      </b>
      <blockquote class="hint info vm-version-notes"><ul><li>Requires <a href="https://downloads.veertu.com/anka/MobileDevice-26.4.pkg" target="_blank" rel="noopener">MobileDevice-26.4.pkg</a> to be installed on the host.</li><li>Requires setting a disk size of 50GB or more.</li></ul></blockquote>
    </td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle">
      <b>
        macOS 26.4.1 (25E253)
        <a href="https://updates.cdn-apple.com/2026WinterFCS/fullrestores/122-28781/DCB2FF13-06CB-44C2-BCA2-DFCAF3521D46/UniversalMac_26.4.1_25E253_Restore.ipsw" title="Download IPSW" style="margin-left: 8px; text-decoration: none;" target="_blank" rel="noopener">
          <span class="fa fa-download" style="font-size:1.2em;"></span>
        </a>
        <button type="button" class="vm-version-info-btn" aria-label="Version requirements">&#8505;</button>
      </b>
      <blockquote class="hint info vm-version-notes"><ul><li>Released 2026-04-09.</li><li>Requires <a href="https://downloads.veertu.com/anka/MobileDevice-26.4.pkg" target="_blank" rel="noopener">MobileDevice-26.4.pkg</a> to be installed on the host.</li><li>Requires setting a disk size of 50GB or more.</li></ul></blockquote>
    </td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle">
      <b>
        macOS 26.4 (25E246)
        <a href="https://updates.cdn-apple.com/2026WinterFCS/fullrestores/122-00766/062A6121-2ABE-45D7-BCB1-72B666B6D2C2/UniversalMac_26.4_25E246_Restore.ipsw" title="Download IPSW" style="margin-left: 8px; text-decoration: none;" target="_blank" rel="noopener">
          <span class="fa fa-download" style="font-size:1.2em;"></span>
        </a>
        <button type="button" class="vm-version-info-btn" aria-label="Version requirements">&#8505;</button>
      </b>
      <blockquote class="hint info vm-version-notes"><ul><li>Released 2026-03-24.</li><li>Requires <a href="https://downloads.veertu.com/anka/MobileDevice-26.4.pkg" target="_blank" rel="noopener">MobileDevice-26.4.pkg</a> to be installed on the host.</li><li>Requires setting a disk size of 50GB or more.</li></ul></blockquote>
    </td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle">
      <b>
        macOS 26.3.2 (25D2140)
        <a href="https://updates.cdn-apple.com/2026WinterFCS/fullrestores/047-94879/40A2B65E-4E49-4EAA-8BEC-62A305007488/UniversalMac_26.3.2_25D2140_Restore.ipsw" title="Download IPSW" style="margin-left: 8px; text-decoration: none;" target="_blank" rel="noopener">
          <span class="fa fa-download" style="font-size:1.2em;"></span>
        </a>
        <button type="button" class="vm-version-info-btn" aria-label="Version requirements">&#8505;</button>
      </b>
      <blockquote class="hint info vm-version-notes"><ul><li>Requires setting a disk size of 50GB or more.</li></ul></blockquote>
    </td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle">
      <b>
        macOS 26.3.1 (25D2128)
        <a href="https://updates.cdn-apple.com/2026WinterFCS/fullrestores/047-88313/2E098049-1731-4415-A206-546D09301973/UniversalMac_26.3.1_25D2128_Restore.ipsw" title="Download IPSW" style="margin-left: 8px; text-decoration: none;" target="_blank" rel="noopener">
          <span class="fa fa-download" style="font-size:1.2em;"></span>
        </a>
        <button type="button" class="vm-version-info-btn" aria-label="Version requirements">&#8505;</button>
      </b>
      <blockquote class="hint info vm-version-notes"><ul><li>Requires setting a disk size of 50GB or more.</li><li>15.x host: Requires Xcode 26.2 or later on the host, fully set up (install all packages) and accepting the license.</li></ul></blockquote>
    </td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle">
      <b>
        macOS 26.3 (25D125)
        <a href="https://updates.cdn-apple.com/2026WinterFCS/fullrestores/047-60229/6D5DBEA5-75A0-4BEF-ACC9-5ACF9B8DF6B7/UniversalMac_26.3_25D125_Restore.ipsw" title="Download IPSW" style="margin-left: 8px; text-decoration: none;" target="_blank" rel="noopener">
          <span class="fa fa-download" style="font-size:1.2em;"></span>
        </a>
        <button type="button" class="vm-version-info-btn" aria-label="Version requirements">&#8505;</button>
      </b>
      <blockquote class="hint info vm-version-notes"><ul><li>Requires setting a disk size of 50GB or more.</li><li>15.x host: Requires Xcode 26.2 or later on the host, fully set up (install all packages) and accepting the license.</li></ul></blockquote>
    </td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle">
      <b>
        macOS 26.2 (25C56)
        <a href="https://updates.cdn-apple.com/2025FallFCS/fullrestores/093-37399/E144C918-CF99-4BBC-B1D0-3E739B9A3F2D/UniversalMac_26.2_25C56_Restore.ipsw" title="Download IPSW" style="margin-left: 8px; text-decoration: none;" target="_blank" rel="noopener">
          <span class="fa fa-download" style="font-size:1.2em;"></span>
        </a>
        <button type="button" class="vm-version-info-btn" aria-label="Version requirements">&#8505;</button>
      </b>
      <blockquote class="hint info vm-version-notes"><ul><li>Requires setting a disk size of 50GB or more.</li><li>15.x host: Requires Xcode 26 or later on the host, fully set up (install all packages) and accepting the license.</li></ul></blockquote>
    </td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle">
      <b>
        macOS 26.1 (25B78)
        <a href="https://updates.cdn-apple.com/2025FallFCS/fullrestores/089-04148/791B6F00-A30B-4EB0-B2E3-257167F7715B/UniversalMac_26.1_25B78_Restore.ipsw" title="Download IPSW" style="margin-left: 8px; text-decoration: none;" target="_blank" rel="noopener">
          <span class="fa fa-download" style="font-size:1.2em;"></span>
        </a>
        <button type="button" class="vm-version-info-btn" aria-label="Version requirements">&#8505;</button>
      </b>
      <blockquote class="hint info vm-version-notes"><ul><li>Requires setting a disk size of 50GB or more.</li><li>15.x host: Requires Xcode 26 or later on the host, fully set up (install all packages) and accepting the license.</li></ul></blockquote>
    </td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle">
      <b>
        macOS 26.0.1 (25A362)
        <a href="https://updates.cdn-apple.com/2025FallFCS/fullrestores/093-50898/60AE7E97-3E60-441B-9B34-E603C694C5C1/UniversalMac_26.0.1_25A362_Restore.ipsw" title="Download IPSW" style="margin-left: 8px; text-decoration: none;" target="_blank" rel="noopener">
          <span class="fa fa-download" style="font-size:1.2em;"></span>
        </a>
        <button type="button" class="vm-version-info-btn" aria-label="Version requirements">&#8505;</button>
      </b>
      <blockquote class="hint info vm-version-notes"><ul><li>Requires setting a disk size of 50GB or more.</li><li>15.x host: Requires Xcode 26 or later on the host, fully set up (install all packages) and accepting the license.</li></ul></blockquote>
    </td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle">
      <b>
        macOS 26.0 (25A354)
        <a href="https://updates.cdn-apple.com/2025FallFCS/fullrestores/093-37622/CE01FAB2-7F26-48EE-AEE4-5E57A7F6D8BB/UniversalMac_26.0_25A354_Restore.ipsw" title="Download IPSW" style="margin-left: 8px; text-decoration: none;" target="_blank" rel="noopener">
          <span class="fa fa-download" style="font-size:1.2em;"></span>
        </a>
        <button type="button" class="vm-version-info-btn" aria-label="Version requirements">&#8505;</button>
      </b>
      <blockquote class="hint info vm-version-notes"><ul><li>Requires setting a disk size of 50GB or more.</li><li>15.x host: Requires Xcode 26 or later on the host, fully set up (install all packages) and accepting the license.</li></ul></blockquote>
    </td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle">
      <b>
        macOS 15.6.1 (24G90)
        <a href="https://updates.cdn-apple.com/2025SummerFCS/fullrestores/093-10809/CFD6DD38-DAF0-40DA-854F-31AAD1294C6F/UniversalMac_15.6.1_24G90_Restore.ipsw" title="Download IPSW" style="margin-left: 8px; text-decoration: none;" target="_blank" rel="noopener">
          <span class="fa fa-download" style="font-size:1.2em;"></span>
        </a>
      </b>
    </td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle">
      <b>
        macOS 15.6 (24G84)
        <a href="https://updates.cdn-apple.com/2025SummerFCS/fullrestores/082-08674/51294E4D-A273-44BE-A280-A69FC347FB87/UniversalMac_15.6_24G84_Restore.ipsw" title="Download IPSW" style="margin-left: 8px; text-decoration: none;" target="_blank" rel="noopener">
          <span class="fa fa-download" style="font-size:1.2em;"></span>
        </a>
      </b>
    </td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 15.5 (24F74)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 15.4.1 (24E263)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 15.4 (24E248)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 15.4 beta (24E5206s)</b></td>
    <td style="font-size: 1.5rem; background-color: #c0392b;">&#10060;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 15.4 beta 3 (24E5228e)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 15.3 beta 3 (24D5055b)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 15.4 beta 2 (24E5222f)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 15.3 beta 2 (24D5040f)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 15.3.2 (24D81)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 15.3.1 (24D70)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 15.3 (24D60)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 15.2 (24C101)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 15.1.1 (24B91)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 15.1.1 (24B2091)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 15.1 (24B2083)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 15.1 (24B83)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 15.0.1 (24A348)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 15.0 (24A335)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 14.6.1 (23G93)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 14.6 (23G80)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 14.5 (23F79)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 14.4.1 (23E224)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 14.3.1 (23D60)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 14.4 (23E214)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 14.3 (23D56)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 14.2.1 (23C71)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 14.1.2 (23B92)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 14.2 (23C64)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 14.1 (23B74)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 14.1.1 (23B81)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 13.6 (22G120)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 14.0 (23A344)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 13.5.1 (22G90)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 13.5.2 (22G91)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 13.5 (22G74)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 13.4.1 (22F82)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 13.4 (22F66)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 13.3.1 (22E261)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 13.3 (22E252)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 13.2.1 (22D68)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 13.2 (22D49)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 13.1 (22C65)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 13.0.1 (22A400)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 13.0 (22A380)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 12.6.1 (21G217)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 12.6 (21G115)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 12.5.1 (21G83)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 12.5 (21G72)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 12.4 (21F79)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 12.4 (21F2081)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 12.3.1 (21E258)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 12.3 (21E230)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 12.2.1 (21D62)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 12.2 (21D49)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 12.0.1 (21A559)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 12.1 (21C52)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
</tbody>
</table>
</div>


<div>
<h4 style="padding: 10px;">Anka 3 (amd64/intel)</h4>
<table>
<tbody style="text-align:center">
  <tr>
    <td style="vertical-align: middle"><b>macOS 26.5.1 (25F80)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 26.5 (25F71)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 26.3.2 (25D2140)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 26.3.1 (25D2128)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 26.1 (25B78)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 26.0.1 (25A362)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 26.0 (25A354)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 15.6.1 (24G90)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 15.6 (24G84)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 15.5 (24F74)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 15.4.1 (24E263)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 15.4 (24E248)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 15.3.2 (24D2082)</b></td>
    <td style="font-size: 1.5rem; background-color: #c0392b;">&#10060;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 15.3.2 (24D81)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 15.3.1 (24D70)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 15.3 (24D60)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 15.2 (24C101)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 15.1.1 (24B91)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 15.1.1 (24B2091)</b></td>
    <td style="font-size: 1.5rem; background-color: #c0392b;">&#10060;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 14.7.4 (23H420)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 14.7.3 (23H417)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 14.7.2 (23H311)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 13.7.4 (22H420)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 13.7.3 (22H417)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 13.7.2 (22H313)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 12.7.4 (21H1123)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <!-- <tr>
    <td style="vertical-align: middle"><b>macOS 11.7.10 (20G1427)</b></td>
    <td style="font-size: 1.5rem; background-color:rgb(238, 179, 18);">&#9989;</td>
  </tr> -->
  <tr>
    <td colspan="2" style="vertical-align: middle">
      <b>
        Older macOS versions (back to 10.x)
        <button type="button" class="vm-version-info-btn" aria-label="Version requirements">&#8505;</button>
      </b>
      <blockquote class="hint info vm-version-notes is-open"><ul><li>Older macOS versions back to 10.x are also functional, but we can't offer guaranteed support for them (neither can Apple).</li><li>10.x VM creation requires installing a special kext in the VM for networking to function:<pre><code>curl -L https://downloads.veertu.com/anka/virtio-net.kext.tar -o /tmp/virtio-net.kext.tar
anka stop {vm}
anka modify VM set custom-variable hw.virtio-net.msix 0
anka modify VM set custom-variable hw.virtio-net.vid 7582
anka start {vm}
anka cp -a /tmp/virtio-net.kext.tar {vm}:/tmp/
anka run {vm} bash -c "tar -xvf /tmp/virtio-net.kext.tar -C /Library/Extensions"
anka run {vm} bash -c "sudo kextload /Library/Extensions/virtio-net.kext"
anka run {vm} bash -c "sudo touch /Library/Extensions"
anka run {vm} bash -c "sudo rm -f /tmp/virtio-net.kext.tar"</code></pre></li></ul></blockquote>
    </td>
  </tr>
</tbody>
</table>
</div>
</div>
{{< /rawhtml >}}
