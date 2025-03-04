<!-- DEBUG START -->
<p style="color: red; font-weight: bold;">DEBUG: "{{ . }}"</p>
<!-- DEBUG END -->

{{ $content := trim .Text " \n" }}

{{ if hasPrefix $content "[!" }}
  {{ $firstLine := index (split $content "\n") 0 }}
  {{ $rest := after 1 (split $content "\n") | delimit "\n" }}

  {{ $type := lower (index (findRE `^\[!(\w+)` $firstLine 1) 0) }}
  {{ $metadata := "" }}

  {{ if findRE `\|` $firstLine }}
    {{ $metadata = replaceRE `^\[!\w+\|` "" $firstLine }}
    {{ $metadata = replaceRE `\].*$` "" $metadata }}
  {{ end }}

  <div class="callout" data-callout="{{ $type }}" {{ if $metadata }}data-callout-metadata="{{ $metadata }}"{{ end }} data-callout-fold="">
      <div class="callout-title">
          <div class="callout-icon"></div>
          <div class="callout-title-inner">{{ if $metadata }}{{ $metadata }}{{ else }}{{ $type | title }}{{ end }}</div>
      </div>
      <div class="callout-content">
          {{ $rest | safeHTML }}
      </div>
  </div>
{{ else }}
  <blockquote>{{ .Text }}</blockquote>
{{ end }}
