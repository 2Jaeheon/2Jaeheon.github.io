{{ $item := .item }}
{{ $fill_image := .config.fill_image | default true }}

{{ $resource := partial "functions/get_featured_image.html" $item }}
{{ $anchor := $item.Params.image.focal_point | default "Center" }}

{{ $link := $item.Params.external_link | default $item.RelPermalink }}
{{ $target := "" }}
{{ if $item.Params.external_link }}
  {{ $link = $item.Params.external_link }}
  {{ $target = "target=\"_blank\" rel=\"noopener\"" }}
{{ end }}

<!-- 카드 레이아웃 -->
<div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-6">
  <div class="group cursor-pointer">

    {{ with $resource }}
    {{ $image := "" }}
    {{ if $fill_image }}
      {{ $image = .Fill (printf "960x540 %s" $anchor) }}
    {{ else }}
      {{ $image = .Fit (printf "960x540 %s" $anchor) }}
    {{ end }}
    {{ if ne $image.MediaType.SubType "gif" }}{{ $image = $image.Process "webp" }}{{ end }}
    <div class="overflow-hidden rounded-md bg-gray-100 transition-all hover:scale-105 dark:bg-gray-800">
      <!-- 이미지 영역 -->
      <a class="block aspect-video" href="{{ $link }}" {{ $target | safeHTMLAttr }}>
        <img alt="{{ $item.Title | plainify }}" class="object-cover w-full h-auto"
          decoding="async" loading="lazy" src="{{ $image.RelPermalink }}">
      </a>
    </div>
    {{ end }}

    <!-- 텍스트 영역 -->
    <div class="mt-3 text-center">
      <h2 class="text-lg font-semibold leading-snug tracking-tight mt-2 dark:text-white">
        <a href="{{ $link }}" {{ $target | safeHTMLAttr }} class="hover:text-primary-500 dark:hover:text-primary-300">
          {{ $item.Title }}
        </a>
      </h2>
      <p class="mt-2 text-sm text-gray-500 dark:text-gray-400">{{ ($item.Params.summary | default $item.Summary) | plainify | htmlUnescape | chomp }}</p>
    </div>

  </div>
</div>