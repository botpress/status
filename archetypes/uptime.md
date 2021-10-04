{{- if .Site.Data.uptimes }}
{{- $disruptedServices := slice }}
{{- range $key, $system := .Site.Params.systems }}
    {{- $uptime := index $.Site.Data.uptimes $system.name }}
    {{- if and $uptime (ne $uptime.state "ok") }}
        {{- $disruptedServices = $disruptedServices | append $uptime.name }}
    {{- end }}
{{- end }}
{{- if $disruptedServices -}}
---
title: Monitoring Disruption Detected
date: {{ now.UTC.Format "2006-01-02T15:04:05" }}
resolved: false
severity: disrupted
affected:
{{- range $key, $value := $disruptedServices }}
- {{ $value }}
{{- end }}
section: issue
---

The automatic uptime monitoring detected a disruption of service.

{{- end }}
{{- end }}
